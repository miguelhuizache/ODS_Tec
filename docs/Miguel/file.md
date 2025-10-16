**Miguel Huizache Vázquez**
**Matricula: A01713936**

----------

**Código:**

import tkinter as tk
from tkinter import ttk, messagebox
from datetime import datetime
import requests
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg

BG_APP = "#f6f7fb"     
BG_AX  = "#eef2f7"  
FG_TXT = "#0f172a"    


LINE_COLOR = "#10b981"   
FILL_COLOR = "#a7f3d0" 
BAR_COLOR  = "#60a5fa"
BAR_EDGE   = "#1d4ed8" 


def fetch_data():
    """
    Conecta con la API de Open-Meteo y obtiene temperaturas horarias
    de Cortazar, Gto (últimas 24 horas).
    Devuelve dos listas: horas (strings cortos) y temperaturas (floats).
    """
    try:
        # Cortazar mi ciudad
        url = "https://api.open-meteo.com/v1/forecast"
        params = {
            "latitude": 20.485,
            "longitude": -100.963,
            "hourly": "temperature_2m",
            "past_days": 1,
            "timezone": "America/Mexico_City",
        }
        response = requests.get(url, params=params, timeout=15)
        response.raise_for_status()
        data = response.json()

        horas_raw = data["hourly"]["time"]
        temperaturas = data["hourly"]["temperature_2m"]

        # Etiquetas más compactas: "dd/HHh"
        horas = []
        for t in horas_raw:
            dt = datetime.fromisoformat(t)
            horas.append(dt.strftime("%d/%Hh"))

        return horas, temperaturas

    except Exception as e:
        messagebox.showerror("Error", f"No se pudieron obtener los datos:\n{e}")
        return [], []


def create_line_chart(horas, temps):
    """Gráfica tipo área (suave) para que se vea diferente pero simple."""
    fig, ax = plt.subplots(figsize=(6, 3))
    fig.patch.set_facecolor(BG_APP)
    ax.set_facecolor(BG_AX)

    x = list(range(len(horas)))  # usar índice para ticks limpios
    ax.plot(x, temps, linestyle="-", marker=None, linewidth=1.8, color=LINE_COLOR)
    ax.fill_between(x, temps, alpha=0.35, color=FILL_COLOR)

    ax.set_title("Temperatura en Cortazar, Gto — área", color=FG_TXT)
    ax.set_xlabel("Hora", color=FG_TXT)
    ax.set_ylabel("°C", color=FG_TXT)

    # ticks: cada 3 horas para no saturar
    step = max(1, len(horas) // 16) 
    ax.set_xticks(x[::step], [horas[i] for i in range(0, len(horas), step)], rotation=45, ha="right")

    for spine in ax.spines.values():
        spine.set_visible(False)

    fig.tight_layout()
    return fig


def create_bar_chart(horas, temps):
    """Gráfica de barras con estilo distinto (borde y leve transparencia)."""
    fig, ax = plt.subplots(figsize=(6, 3))
    fig.patch.set_facecolor(BG_APP)
    ax.set_facecolor(BG_AX)

    x = list(range(len(horas)))
    ax.bar(x, temps, alpha=0.85, color=BAR_COLOR, edgecolor=BAR_EDGE, linewidth=0.6)

    ax.set_title("Temperatura en Cortazar, Gto — barras", color=FG_TXT)
    ax.set_xlabel("Hora", color=FG_TXT)
    ax.set_ylabel("°C", color=FG_TXT)

    step = max(1, len(horas) // 16)
    ax.set_xticks(x[::step], [horas[i] for i in range(0, len(horas), step)], rotation=45, ha="right")

    for spine in ax.spines.values():
        spine.set_visible(False)

    fig.tight_layout()
    return fig


def mostrar_graficas(container, horas, temps):
    """
    Inserta las gráficas en un sub-frame y limpia las anteriores
    para que no se acumulen si le vuelves a dar click al botón.
    """
    # Limpiar gráficos anteriores
    for child in container.winfo_children():
        child.destroy()

    # Área
    fig1 = create_line_chart(horas, temps)
    canvas1 = FigureCanvasTkAgg(fig1, master=container)
    canvas1.draw()
    canvas1.get_tk_widget().pack(pady=10, fill="x")

    # Barras
    fig2 = create_bar_chart(horas, temps)
    canvas2 = FigureCanvasTkAgg(fig2, master=container)
    canvas2.draw()
    canvas2.get_tk_widget().pack(pady=10, fill="x")


def open_win_canvas(parent: tk.Tk):
    """
    Crea la ventana secundaria con gráficas de la API.
    """
    win = tk.Toplevel(parent)
    win.title("Clima simple — Cortazar, Gto")
    win.geometry("960x900")
    win.configure(bg=BG_APP)

    frm = ttk.Frame(win, padding=12)
    frm.pack(fill="both", expand=True)


    plots_container = ttk.Frame(frm)
    plots_container.pack(fill="both", expand=True)

    def cargar():
        horas, temps = fetch_data()
        if horas and temps:
            mostrar_graficas(plots_container, horas, temps)

    ttk.Button(frm, text="Cargar y mostrar gráficas", command=cargar).pack(pady=10)


if __name__ == "__main__":
    root = tk.Tk()
    root.title("Prueba win_canvas")
    root.configure(bg=BG_APP)
    ttk.Button(root, text="Abrir ventana Canvas", command=lambda: open_win_canvas(root)).pack(pady=20)
    root.mainloop()
    
-----------
**Imagen del resultado:**
![Imagen de WhatsApp 2025-10-16 a las 11 57 18_447fd7c5](https://github.com/user-attachments/assets/97159db2-96b8-4dd7-91fc-077bd11ea229)
---------

**Cambios que hice:**
Ciudad: pasé de León a Cortazar, Gto. Metí las coords 20.485, -100.963 y dejé el timezone fijo en America/Mexico_City para que no ande brincando nada. Colores/estilo: cambié la paleta: fondos más claritos, área en verde y barras azules con borde. Misma estructura, vibe distinta. Otra vista: la primera gráfica ahora es de área (línea + relleno). Mismo dato, se siente más limpia y moderna. Horas: acorté las etiquetas a dd/HHh y puse menos ticks (aprox cada 3 h) pa’ que no se vea atascado. Sin duplicados: si vuelves a dar al botón, limpio y repinto; ya no se acumulan las gráficas. Limpieza visual: quité bordes de ejes y unifiqué fondos/títulos/labels para que todo se lea más fácil.


