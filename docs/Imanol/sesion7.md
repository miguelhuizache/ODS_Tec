# Gráficas modificadas sesión 7

## 📊 Visualización

![Gráficas de sesión 7](<img width="2450" height="1653" alt="image" src="https://github.com/user-attachments/assets/b33070f0-f08a-45a3-ac40-59a112f733c5" />)

## 🐍 Código en Python

```python
import tkinter as tk
from tkinter import ttk, messagebox
import requests
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg


def fetch_data():
    """
    Conecta con la API de Open-Meteo y obtiene temperaturas horarias
    de Ciudad de México (últimas 24 horas).
    Devuelve dos listas: horas y temperaturas.
    """
    try:
        url = (
            "https://api.open-meteo.com/v1/forecast"
            "?latitude=19.4326&longitude=-99.1332"
            "&hourly=temperature_2m,relativehumidity_2m,windspeed_10m&past_days=1"
            "&timezone=auto"
        )
        response = requests.get(url, timeout=15)
        response.raise_for_status()
        data = response.json()

        horas = data["hourly"]["time"]
        temperaturas = data["hourly"]["temperature_2m"]
        humedad = data["hourly"]["relativehumidity_2m"]
        viento = data["hourly"]["windspeed_10m"]

        return horas, temperaturas, humedad, viento
    except Exception as e:
        messagebox.showerror("Error", f"No se pudieron obtener los datos:\\n{e}")
        return [], []


def create_line_chart(horas, temps, label=None, color=None):
    """Gráfica de línea."""
    fig, ax = plt.subplots(figsize=(6, 3))
    plot_args = {"linestyle": "-", "marker": "s", "markersize": 4, "linewidth": 2, "alpha": 0.7}
    if color:
        plot_args["color"] = color
    ax.plot(horas, temps, **plot_args)
    if label:
        ax.set_title(label)
        ax.set_ylabel(label)
    else:
        ax.set_title("Ciudad de México (línea)")
        ax.set_ylabel("")
    ax.set_xlabel("Hora")
    # Mostrar solo cada n-ésima etiqueta en el eje X
    ax.set_xticks(horas)
    ax.tick_params(axis="x", rotation=45)
    ax.grid(True, linestyle="--", alpha=.5)
    fig.tight_layout()
    return fig


def create_bar_chart(horas, temps, label=None, color=None):
    """Gráfica de barras."""
    fig, ax = plt.subplots(figsize=(6, 3))
    bar_args = {"alpha": 0.7}
    if color:
        bar_args["color"] = color
    ax.bar(horas, temps, **bar_args)
    if label:
        ax.set_title(label)
        ax.set_ylabel(label)
    else:
        ax.set_title("Ciudad de México (barras)")
        ax.set_ylabel("")
    ax.set_xlabel("Hora")
    ax.set_xticks(horas)
    ax.tick_params(axis="x", rotation=45)
    ax.grid(True, linestyle="--", alpha=.5)
    fig.tight_layout()
    return fig


def mostrar_graficas(frm, horas, temps):
    from datetime import datetime
    def obtener_fecha(fecha_iso):
        try:
            dt = datetime.strptime(fecha_iso, "%Y-%m-%dT%H:%M")
            return dt.strftime("%d %b %Y")
        except Exception:
            return fecha_iso
    def obtener_hora(fecha_iso):
        try:
            dt = datetime.strptime(fecha_iso, "%Y-%m-%dT%H:%M")
            return dt.strftime("%H:%M")
        except Exception:
            return fecha_iso
    """Inserta las tres gráficas en el frame de la ventana tkinter."""
    # Crear frames para cuadrícula de gráficas
    grid_frame = ttk.Frame(frm)
    grid_frame.pack(pady=10, fill="x")

    # Graficas en cuadrícula 2x3
    # Graficas en cuadrícula 2x3 con nombres y colores específicos, solo primeros 24 datos
    horas_24 = horas[:24]
    temps_24 = temps[:24]
    humedad_24 = frm.humedad[:24]
    viento_24 = frm.viento[:24]
    # Extraer la fecha del primer dato
    fecha_grafica = obtener_fecha(horas_24[0]) if horas_24 else ""
    horas_x = [obtener_hora(h) for h in horas_24]

    canvas1 = FigureCanvasTkAgg(create_line_chart(horas_x, temps_24, f"Temperatura (°C) - Línea ({fecha_grafica})", "red"), master=grid_frame)
    canvas1.draw()
    canvas1.get_tk_widget().grid(row=0, column=0, padx=5, pady=5, sticky="nsew")

    canvas2 = FigureCanvasTkAgg(create_bar_chart(horas_x, temps_24, f"Temperatura (°C) - Barras ({fecha_grafica})", "red"), master=grid_frame)
    canvas2.draw()
    canvas2.get_tk_widget().grid(row=0, column=1, padx=5, pady=5, sticky="nsew")

    canvas3 = FigureCanvasTkAgg(create_line_chart(horas_x, humedad_24, f"Humedad relativa (%) - Línea ({fecha_grafica})", "blue"), master=grid_frame)
    canvas3.draw()
    canvas3.get_tk_widget().grid(row=1, column=0, padx=5, pady=5, sticky="nsew")

    canvas4 = FigureCanvasTkAgg(create_bar_chart(horas_x, humedad_24, f"Humedad relativa (%) - Barras ({fecha_grafica})", "blue"), master=grid_frame)
    canvas4.draw()
    canvas4.get_tk_widget().grid(row=1, column=1, padx=5, pady=5, sticky="nsew")

    canvas5 = FigureCanvasTkAgg(create_line_chart(horas_x, viento_24, f"Velocidad del viento (m/s) - Línea ({fecha_grafica})", "green"), master=grid_frame)
    canvas5.draw()
    canvas5.get_tk_widget().grid(row=2, column=0, padx=5, pady=5, sticky="nsew")

    canvas6 = FigureCanvasTkAgg(create_bar_chart(horas_x, viento_24, f"Velocidad del viento (m/s) - Barras ({fecha_grafica})", "green"), master=grid_frame)
    canvas6.draw()
    canvas6.get_tk_widget().grid(row=2, column=1, padx=5, pady=5, sticky="nsew")

    # Nombres y tablas
    tabla_temp_lbl = ttk.Label(frm, text="Tabla de Temperatura (°C)", font=("Arial", 12, "bold"))
    tabla_temp_lbl.pack(pady=(15, 0))
    tabla_temp = ttk.Treeview(frm, columns=("hora", "temp"), show="headings", height=8)
    tabla_temp.heading("hora", text="Hora")
    tabla_temp.heading("temp", text="Temperatura (°C)")
    from datetime import datetime
    def formato_legible(fecha_iso):
        try:
            dt = datetime.strptime(fecha_iso, "%Y-%m-%dT%H:%M")
            return dt.strftime("%d %b %Y, %H:%M")
        except Exception:
            return fecha_iso

    for h, t in zip(horas, temps):
        tabla_temp.insert("", "end", values=(formato_legible(h), t))
    tabla_temp.pack(pady=5, fill="x")

    tabla_hum_lbl = ttk.Label(frm, text="Tabla de Humedad relativa (%)", font=("Arial", 12, "bold"))
    tabla_hum_lbl.pack(pady=(15, 0))
    tabla_hum = ttk.Treeview(frm, columns=("hora", "hum"), show="headings", height=8)
    tabla_hum.heading("hora", text="Hora")
    tabla_hum.heading("hum", text="Humedad relativa (%)")
    for h, hu in zip(horas, frm.humedad):
        tabla_hum.insert("", "end", values=(formato_legible(h), hu))
    tabla_hum.pack(pady=5, fill="x")

    tabla_viento_lbl = ttk.Label(frm, text="Tabla de Velocidad del viento (m/s)", font=("Arial", 12, "bold"))
    tabla_viento_lbl.pack(pady=(15, 0))
    tabla_viento = ttk.Treeview(frm, columns=("hora", "viento"), show="headings", height=8)
    tabla_viento.heading("hora", text="Hora")
    tabla_viento.heading("viento", text="Velocidad del viento (m/s)")
    for h, v in zip(horas, frm.viento):
        tabla_viento.insert("", "end", values=(formato_legible(h), v))
    tabla_viento.pack(pady=5, fill="x")


def open_win_canvas(parent: tk.Tk):
    """
    Crea la ventana secundaria con gráficas de la API.
    """
    win = tk.Toplevel(parent)
    win.title("Canvas con API (Open-Meteo) y gráficas")
    win.geometry("960x1000")

    # Frame principal con scrollbar vertical
    container = ttk.Frame(win, padding=12)
    container.pack(fill="both", expand=True)

    canvas = tk.Canvas(container)
    scrollbar = ttk.Scrollbar(container, orient="vertical", command=canvas.yview)
    scrollable_frame = ttk.Frame(canvas)

    scrollable_frame.bind(
        "<Configure>", lambda e: canvas.configure(scrollregion=canvas.bbox("all"))
    )
    canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")
    canvas.configure(yscrollcommand=scrollbar.set)

    canvas.pack(side="left", fill="both", expand=True)
    scrollbar.pack(side="right", fill="y")

    frm = scrollable_frame

    # Botón para cargar datos y graficar
    def cargar():
        horas, temps, humedad, viento = fetch_data()
        if horas and temps:
            frm.humedad = humedad
            frm.viento = viento
            mostrar_graficas(frm, horas, temps)

    ttk.Button(frm, text="Cargar y mostrar gráficas", command=cargar).pack(pady=10)


# Para pruebas independientes (opcional)
if __name__ == "__main__":
    root = tk.Tk()
    root.title("Prueba win_canvas")
    ttk.Button(root, text="Abrir ventana Canvas", command=lambda: open_win_canvas(root)).pack(pady=20)
    root.mainloop()
