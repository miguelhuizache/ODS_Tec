# --- NutriTec: visor de registros (con información incluida) ---
# Copia y pega este archivo. Muestra datos desde CSV si existe;
# si no, muestra la ficha de NutriTec embebida como fallback.

import tkinter as tk
from tkinter import ttk, messagebox
import csv
from pathlib import Path

def abrir_registros(win: tk.Tk | tk.Toplevel):
    # ===== Estilos =====
    style = ttk.Style()
    style.theme_use("clam")
    style.configure("TFrame", background="white")
    style.configure("TLabel", background="white", foreground="#0a3d62", font=("Segoe UI", 11))
    style.configure("Header.TLabel", font=("Segoe UI", 14, "bold"), foreground="#1e56a0", background="white")
    style.configure("Nav.TButton", font=("Segoe UI", 11, "bold"), padding=6, background="#2980b9", foreground="white")
    style.map("Nav.TButton", background=[("active", "#1e73be")])

    frm = ttk.Frame(win, padding=20, style="TFrame")
    frm.pack(fill="both", expand=True)

    # ===== Intentar cargar CSV =====
    # Ajusta esta ruta a tu estructura real si es necesario.
    try:
        ruta = Path(__file__).resolve().parents[2] / "data" / "form_data.csv"
    except Exception:
        ruta = Path.cwd() / "data" / "form_data.csv"

    reader = []
    if ruta.exists():
        try:
            with open(ruta, "r", encoding="utf-8", newline="") as f:
                reader = list(csv.DictReader(f))
        except Exception as e:
            # Si falla la lectura, seguimos con el fallback embebido
            print(f"Advertencia: no se pudo leer el CSV ({e}). Se usará la información embebida.")

    # ===== Fallback embebido (información de NutriTec) =====
    if not reader:
        # Un solo "registro" con toda tu ficha en formato clave: valor
        reader = [{
            "Nombre de la app": "NutriTec",
            "Descripción (1 línea)": "App que registra alimentación y actividad física y muestra progreso semanal.",
            "Problema que resuelve": "Falta de herramientas simples y motivantes para que jóvenes lleven control básico de nutrición y actividad física.",
            "Usuario objetivo": "Menores de 18 años y principiantes que buscan hábitos saludables sin complejidad.",
            "Propuesta de valor": "Registro ultra rápido (<30 s), visualización semanal clara y retroalimentación sencilla.",
            "MVP - Funciones clave": "Registro de comidas y actividad; Gráfica semanal; Persistencia local; Flujo < 30 s.",
            "MVP - Criterios de éxito": "≥95% registros exitosos; gráfica visible; datos persisten; registro por novatos en <30 s.",
            "Flujo básico de usuario": "1) Abrir app; 2) +Registro (Comida/Actividad); 3) Ingresar dato y Guardar; 4) Ver gráfica semanal.",
            "Métricas iniciales": "Días con ≥1 registro/semana; % sesiones con registro; tiempo medio <30 s; semanas consecutivas activas.",
            "Contenido/UX": "Etiquetas simples; plantillas rápidas; mensajes de refuerzo (p.ej., “¡3 días seguidos!”).",
            "Tecnologías sugeridas": "Tkinter / Web (HTML+JS) / Flutter; JSON/CSV o localStorage; Matplotlib o Chart.js/Recharts.",
            "Estructura de repo (sugerida)": "nutritec/src/... (app.py, models, views, controllers, storage) + assets + data.",
            "Roadmap corto": "S1: formulario+guardado; S2: gráfica+dashboard; S3: UX+mensajes+pruebas.",
            "Política de datos (mínima)": "Datos locales; no se comparten; opción de exportar/borrar.",
            "Pitch (30 s)": "NutriTec permite registrar comidas y actividad en <30 s y ver progreso semanal localmente.",
            "Estado/pendientes": "[ ] Persistencia | [ ] Gráfica semanal | [ ] Validaciones | [ ] Exportar CSV/JSON",
            "Repositorio": "(Agrega aquí el link de GitHub cuando lo tengas)"
        }]

    # ===== UI de registros =====
    idx = tk.IntVar(value=0)
    record_frame = ttk.Frame(frm, style="TFrame")
    record_frame.pack(fill="both", expand=True)

    def mostrar_registro():
        for widget in record_frame.winfo_children():
            widget.destroy()
        r = reader[idx.get()]
        ttk.Label(
            record_frame,
            text=f"Registro {idx.get()+1}/{len(reader)}",
            style="Header.TLabel"
        ).pack(pady=10)

        # Mostrar cada par clave: valor en filas
        for key, val in r.items():
            row = ttk.Frame(record_frame, style="TFrame")
            row.pack(fill="x", pady=2)
            ttk.Label(row, text=f"{key}:", width=28, anchor="w", style="TLabel").pack(side="left")
            ttk.Label(row, text=(str(val) if (val is not None and val != '') else "-"), anchor="w", style="TLabel", wraplength=720, justify="left").pack(side="left", fill="x", expand=True)

    def siguiente():
        if idx.get() < len(reader) - 1:
            idx.set(idx.get() + 1)
            mostrar_registro()

    def anterior():
        if idx.get() > 0:
            idx.set(idx.get() - 1)
            mostrar_registro()

    btn_frame = ttk.Frame(frm, style="TFrame")
    btn_frame.pack(pady=10)
    ttk.Button(btn_frame, text="Anterior", command=anterior, style="Nav.TButton").grid(row=0, column=0, padx=8)
    ttk.Button(btn_frame, text="Siguiente", command=siguiente, style="Nav.TButton").grid(row=0, column=1, padx=8)
    ttk.Button(btn_frame, text="Cerrar", command=win.destroy, style="Nav.TButton").grid(row=0, column=2, padx=8)

    mostrar_registro()

# Llama a la vista desde tu ventana principal con:
# abrir_registros(win)
