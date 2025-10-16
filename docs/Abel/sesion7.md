

```python
# open_meteo_queretaro_v1.py
# Interfaz similar a tu captura: combos (parámetro/rango/color), checkbox de relleno, botón y 2 gráficas.
# Requiere: requests, matplotlib, tkinter

import tkinter as tk
from tkinter import ttk, messagebox
from datetime import datetime
import requests
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg

APP_TITLE = "Open-Meteo | Querétaro – versión V1 (diseño distinto)"

# -------- Config UI --------
BG_APP = "#f6f7fb"
BG_AX  = "#eaf0f6"
FG_TXT = "#0f172a"

# colores disponibles en el combo (clave -> matplotlib color)
COLOR_MAP = {
    "Azul":  "#1f77b4",
    "Verde": "#2ca02c",
    "Naranja": "#ff7f0e",
    "Morado": "#9467bd",
    "Rojo": "#d62728",
    "Gris": "#7f7f7f",
}

# -------- Open-Meteo helpers --------
BASE_URL = "https://api.open-meteo.com/v1/forecast"

# Querétaro centro aprox.
LAT, LON = 20.59, -100.39
TIMEZONE = "America/Mexico_City"

# map de rango mostrado en la UI -> past_days para Open-Meteo
RANGO_TO_PAST_DAYS = {
    "Últimas 24 h": 1,
    "Últimas 72 h": 3,
    "Últimos 7 días": 7,
}

# parámetros válidos para hourly (puedes agregar más)
PARAMS_HOURLY = {
    "temperature_2m": "Temperatura a 2 m (°C)",
    # "relativehumidity_2m": "Humedad relativa a 2 m (%)",   # ejemplo si quieres extender
    # "apparent_temperature": "Sensación térmica (°C)",
}

def fetch_hourly(hourly_key: str, past_days: int):
    """Obtiene series horarias desde Open-Meteo."""
    try:
        resp = requests.get(
            BASE_URL,
            params={
                "latitude": LAT,
                "longitude": LON,
                "hourly": hourly_key,
                "past_days": past_days,
                "timezone": TIMEZONE,
            },
            timeout=15,
        )
        resp.raise_for_status()
        js = resp.json()
        horas_iso = js["hourly"]["time"]
        valores = js["hourly"][hourly_key]

        horas_fmt = [datetime.fromisoformat(t).strftime("%d/%H:%M") for t in horas_iso]
        return horas_fmt, valores
    except Exception as e:
        messagebox.showerror("Error", f"No se pudieron obtener los datos:\n{e}")
        return [], []

# -------- Gráficas --------
def plot_line(container, horas, vals, color, fill_area: bool, y_label: str):
    fig, ax = plt.subplots(figsize=(8, 3))
    fig.patch.set_facecolor(BG_APP)
    ax.set_facecolor(BG_AX)

    x = list(range(len(horas)))
    ax.plot(x, vals, marker="o", linewidth=1.8, color=color)
    if fill_area:
        ax.fill_between(x, vals, alpha=0.25, color=color)

    ax.set_title("Temperatura en Querétaro (línea)", color=FG_TXT)
    ax.set_xlabel("Hora", color=FG_TXT)
    ax.set_ylabel("°C", color=FG_TXT)

    step = max(1, len(horas)//12)  # espaciado en eje x
    ax.set_xticks(x[::step], [horas[i] for i in range(0, len(horas), step)], rotation=45, ha="right")

    for s in ax.spines.values():
        s.set_visible(False)
    ax.grid(True, linestyle="--", alpha=0.35)
    fig.tight_layout()

    canvas = FigureCanvasTkAgg(fig, master=container)
    canvas.draw()
    canvas.get_tk_widget().pack(pady=8, fill="x")

def plot_bar(container, horas, vals, color, y_label: str):
    fig, ax = plt.subplots(figsize=(8, 3))
    fig.patch.set_facecolor(BG_APP)
    ax.set_facecolor(BG_AX)

    x = list(range(len(horas)))
    ax.bar(x, vals, color=color, alpha=0.9)

    ax.set_title("Temperatura en Querétaro (barras)", color=FG_TXT)
    ax.set_xlabel("Hora", color=FG_TXT)
    ax.set_ylabel("°C", color=FG_TXT)

    step = max(1, len(horas)//12)
    ax.set_xticks(x[::step], [horas[i] for i in range(0, len(horas), step)], rotation=45, ha="right")

    for s in ax.spines.values():
        s.set_visible(False)
    ax.grid(True, axis="y", linestyle="--", alpha=0.35)
    fig.tight_layout()

    canvas = FigureCanvasTkAgg(fig, master=container)
    canvas.draw()
    canvas.get_tk_widget().pack(pady=8, fill="x")

# -------- UI --------
def main():
    root = tk.Tk()
    root.title(APP_TITLE)
    root.configure(bg=BG_APP)

    # Barra superior de controles
    bar = ttk.Frame(root, padding=(10, 8))
    bar.pack(fill="x")

    ttk.Label(bar, text="Parámetro:").pack(side="left", padx=(0, 6))
    cb_param = ttk.Combobox(bar, state="readonly", width=18,
                            values=list(PARAMS_HOURLY.keys()))
    cb_param.set("temperature_2m")
    cb_param.pack(side="left", padx=(0, 14))

    ttk.Label(bar, text="Rango:").pack(side="left", padx=(0, 6))
    cb_rango = ttk.Combobox(bar, state="readonly", width=14,
                            values=list(RANGO_TO_PAST_DAYS.keys()))
    cb_rango.set("Últimas 24 h")
    cb_rango.pack(side="left", padx=(0, 14))

    ttk.Label(bar, text="Color:").pack(side="left", padx=(0, 6))
    cb_color = ttk.Combobox(bar, state="readonly", width=10,
                            values=list(COLOR_MAP.keys()))
    cb_color.set("Azul")
    cb_color.pack(side="left", padx=(0, 14))

    fill_var = tk.BooleanVar(value=True)
    chk_fill = ttk.Checkbutton(bar, text="Rellenar área (línea)", variable=fill_var)
    chk_fill.pack(side="left", padx=(0, 14))

    plots_area = ttk.Frame(root, padding=(10, 0))
    plots_area.pack(fill="both", expand=True)

    def cargar():
        # limpiar plots previos
        for w in plots_area.winfo_children():
            w.destroy()

        hourly_key = cb_param.get()
        past_days = RANGO_TO_PAST_DAYS[cb_rango.get()]
        color = COLOR_MAP[cb_color.get()]
        fill_area = fill_var.get()

        horas, vals = fetch_hourly(hourly_key, past_days)
        if not horas:
            return

        y_label = "°C" if "temperature" in hourly_key else ""
        plot_line(plots_area, horas, vals, color, fill_area, y_label)
        plot_bar(plots_area, horas, vals, color, y_label)

    ttk.Button(bar, text="Cargar y mostrar gráficas", command=cargar).pack(side="left")

    root.geometry("1000x680")
    root.mainloop()

if __name__ == "__main__":
    main()
```

<img width="1012" height="726" alt="image" src="https://github.com/user-attachments/assets/e4a3d363-e419-43d6-8ffd-ece3bea6a211" />

