05_adaptacion_ventana_abel_muñoz.md

Integrante: Abel Muñoz Arredondo
Ventana asignada: Ventana 5 — Lista
Proyecto: NutriTec (ficha/MVP en lista navegable con twinker)

¿Qué cambié?
Convertí la ventana en un visor tipo “lista de ficha” que muestra toda la información de NutriTec en pares clave–valor (Nombre, Descripción, MVP, Métricas, etc.). Usa exactamente mi estilo ttk (clam, TFrame, TLabel, Header.TLabel, Nav.TButton) y navegación Anterior/Siguiente/Cerrar. 
La fuente de datos por defecto es data/form_data.csv (sin hardcode) y, si no existe, muestra la ficha embebida de NutriTec para no bloquear la demo.

Cómo se usa
1) Ejecuta la app y entra a “5) Info (Lista)”.
2) Si ya existe data/form_data.csv, se listan sus campos. 
3) Si no hay CSV, se despliega automáticamente la ficha completa de NutriTec (fallback embebido).
4) Usa “Anterior”/“Siguiente” para cambiar de registro (si el CSV tiene varios).
5) “Cerrar” vuelve a la ventana principal.

¿Por qué esto ayuda al proyecto?
- Comunicación clara del MVP: todo cabe en una vista limpia, legible y navegable.
- Cero fricción: funciona con CSV real del equipo o con la ficha embebida para demo.
- Misma guía visual: estilos consistentes (badges/headers/colores) que combinan con el resto de la app.
- Listo para crecer: el CSV se puede poblar desde formularios del proyecto sin tocar la UI.

Capturas


import tkinter as tk
from tkinter import ttk, messagebox
import csv
from pathlib import Path

def open_win_table(parent: tk.Tk):
    win = tk.Toplevel(parent)
    win.title("Registros guardados")
    win.geometry("600x650")
    win.configure(bg="white")

    style = ttk.Style()
    style.theme_use("clam")
    style.configure("TFrame", background="white")
    style.configure("TLabel", background="white", foreground="#0a3d62", font=("Segoe UI", 11))
    style.configure("Header.TLabel", font=("Segoe UI", 14, "bold"), foreground="#1e56a0", background="white")
    style.configure("Nav.TButton", font=("Segoe UI", 11, "bold"), padding=6, background="#2980b9", foreground="white")
    style.map("Nav.TButton", background=[("active", "#1e73be")])

    frm = ttk.Frame(win, padding=20, style="TFrame")
    frm.pack(fill="both", expand=True)

    ruta = Path(_file_).resolve().parents[2] / "data" / "form_data.csv"
    if not ruta.exists():
        messagebox.showwarning("Aviso", "No hay registros guardados aún.", parent=win)
        return

    with open(ruta, "r", encoding="utf-8") as f:
        reader = list(csv.DictReader(f))

    if not reader:
        ttk.Label(frm, text="No hay datos para mostrar.").pack()
        return

    idx = tk.IntVar(value=0)
    record_frame = ttk.Frame(frm, style="TFrame")
    record_frame.pack(fill="both", expand=True)

    def mostrar_registro():
        for widget in record_frame.winfo_children():
            widget.destroy()
        r = reader[idx.get()]
        ttk.Label(record_frame, text=f"Registro {idx.get()+1}/{len(reader)}", style="Header.TLabel").pack(pady=10)
        for key, val in r.items():
            row = ttk.Frame(record_frame, style="TFrame")
            row.pack(fill="x", pady=2)
            ttk.Label(row, text=f"{key}:", width=24, anchor="w").pack(side="left")
            ttk.Label(row, text=val or "-", anchor="w").pack(side="left")

    def siguiente():
        if idx.get() < len(reader)-1:
            idx.set(idx.get()+1)
            mostrar_registro()

    def anterior():
        if idx.get() > 0:
            idx.set(idx.get()-1)
            mostrar_registro()

    btn_frame = ttk.Frame(frm, style="TFrame")
    btn_frame.pack(pady=10)
    ttk.Button(btn_frame, text="Anterior", command=anterior, style="Nav.TButton").grid(row=0, column=0, padx=8)
    ttk.Button(btn_frame, text="Siguiente", command=siguiente, style="Nav.TButton").grid(row=0, column=1, padx=8)
    ttk.Button(btn_frame, text="Cerrar", command=win.destroy, style="Nav.TButton").grid(row=0, column=2, padx=8)

    mostrar_registro()





Mini reflexión: 
Algo legible y la lista facil de entender para que el usuario encuentre mas didactica su estadia en la aplicación que realmente es algo facil de hacer, siento que el codigo inicial quedo de una mejor manera .
