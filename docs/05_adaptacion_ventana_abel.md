05_adaptacion_ventana_abel_muñoz.md
Integrante: Abel Muñoz Arredondo
Ventana asignada: Ventana 5 — win_info.py
Proyecto: NutriTec (ficha/MVP en lista navegable con ttk)

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

def abrir_info_nutritec(win: tk.Tk | tk.Toplevel):
    # ====== ESTILOS (TUS LÍNEAS) ======
    style = ttk.Style()
    style.theme_use("clam")
    style.configure("TFrame", background="white")
    style.configure("TLabel", background="white", foreground="#0a3d62", font=("Segoe UI", 11))
    style.configure("Header.TLabel", font=("Segoe UI", 14, "bold"), foreground="#1e56a0", background="white")
    style.configure("Nav.TButton", font=("Segoe UI", 11, "bold"), padding=6, background="#2980b9", foreground="white")
    style.map("Nav.TButton", background=[("active", "#1e73be")])

    # ====== CONTENEDOR ======
    frm = ttk.Frame(win, padding=20, style="TFrame")
    frm.pack(fill="both", expand=True)

    # ====== RUTA CSV (igual a tu estructura; usa __file__ correcto) ======
    try:
        ruta = Path(__file__).resolve().parents[2] / "data" / "form_data.csv"
    except Exception:
        ruta = Path.cwd() / "data" / "form_data.csv"

    # ====== LECTURA CSV ======
    reader = []
    if ruta.exists():
        try:
            with open(ruta, "r", encoding="utf-8", newline="") as f:
                reader = list(csv.DictReader(f))
        except Exception as e:
            messagebox.showwarning("Aviso", f"No se pudo leer el CSV.\n{e}", parent=win)
            reader = []

    # ====== FALLBACK: FICHA EMBEBIDA (LA LISTA QUE ME PEDISTE) ======
    if not reader:
        info_pairs = [
            ("Nombre de la app", "NutriTec"),
            ("Descripción (1 línea)", "App que registra alimentación y actividad física y muestra progreso semanal."),
            ("Problema que resuelve", "Falta de herramientas simples y motivantes para que jóvenes lleven control básico de nutrición y actividad física."),
            ("Usuario objetivo", "Menores de 18 años y principiantes que buscan hábitos saludables sin complejidad."),
            ("Propuesta de valor", "Registro ultra rápido (<30 s), visualización semanal clara y retroalimentación sencilla."),
            ("MVP — Funciones clave",
             "- Registro de comidas y actividad física.\n"
             "- Gráfica semanal de progreso (calorías/porciones, minutos activos).\n"
             "- Persistencia local (archivo/localStorage).\n"
             "- Flujo de registro en menos de 30 s."),
            ("MVP — Criterios de éxito",
             "- Tasa de registro exitoso ≥ 95%.\n"
             "- Gráfica semanal visible y entendible.\n"
             "- Datos se conservan al cerrar/abrir la app.\n"
             "- Registro completado por novatos en < 30 s."),
            ("Flujo básico de usuario",
             "1) Abrir app → ver resumen semanal.\n"
             "2) “+ Registro” → elegir Comida o Actividad.\n"
             "3) Ingresar dato rápido (porciones/minutos/notas) → Guardar.\n"
             "4) Volver al dashboard → gráfica semanal actualizada."),
            ("Métricas iniciales",
             "- Días con ≥ 1 registro por semana.\n"
             "- % de sesiones que completan registro.\n"
             "- Tiempo medio de registro (objetivo < 30 s).\n"
             "- Semanas consecutivas activas por usuario."),
            ("Contenido/UX",
             "- Etiquetas simples (desayuno, comida, cena / cardio, fuerza, pasos).\n"
             "- Plantillas de registro rápidas.\n"
             "- Mensajes breves de refuerzo (“¡Bien! Llevas 3 días seguidos.”)."),
            ("Tecnologías sugeridas",
             "- Tkinter / Web (HTML+JS) / Flutter.\n"
             "- Datos locales: JSON/CSV o localStorage/IndexedDB.\n"
             "- Gráficas: Matplotlib o Chart.js/Recharts."),
            ("Estructura de repositorio (sugerencia)",
             "nutritec/\n"
             "- README.md\n"
             "- LICENSE\n"
             "- src/\n"
             "  - app.py (punto de entrada)\n"
             "  - models/ (datos: comidas, actividad)\n"
             "  - views/ (UI)\n"
             "  - controllers/ (lógica)\n"
             "  - storage/ (persistencia json/csv)\n"
             "- assets/ (íconos/imagenes)\n"
             "- data/ (archivos locales si aplica)"),
            ("Roadmap corto",
             "- Semana 1: Formulario de registro + guardado local + lista de registros.\n"
             "- Semana 2: Gráfica semanal + resumen en dashboard.\n"
             "- Semana 3: Pulido de UX + mensajes motivacionales + pruebas con usuarios."),
            ("Política de datos (mínima)",
             "- Datos locales en el dispositivo del usuario.\n"
             "- No se comparten con terceros.\n"
             "- Opción de exportar y borrar datos."),
            ("Pitch (30 s)",
             "“NutriTec es una app ligera que permite registrar comidas y actividad en menos de 30 segundos y ver el progreso semanal en una gráfica clara. Guarda todo de forma local y motiva con pequeños mensajes de avance.”"),
            ("Estado/pendientes",
             "- [ ] Conectar formulario con persistencia\n"
             "- [ ] Generar gráfica semanal\n"
             "- [ ] Validaciones básicas (campos vacíos/formatos)\n"
             "- [ ] Exportar datos (CSV/JSON)"),
            ("Repositorio", "GitHub: (pégalo aquí cuando lo tengas)")
        ]
        reader = [{k: v for k, v in info_pairs}]

    # ====== SI AÚN NO HAY DATOS ======
    if not reader:
        ttk.Label(frm, text="No hay datos para mostrar.", style="TLabel").pack()
        return

    # ====== TU ESTRUCTURA: RECORD + NAV ======
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
            ttk.Label(row, text=f"{key}:", width=28, anchor="w").pack(side="left")
            ttk.Label(row, text=(str(val) if val not in [None, ""] else "-"),
                      anchor="w", wraplength=760, justify="left").pack(side="left")

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

Mini reflexión
Algo legible y la lista facil de entender para que el usuario encuentre mas didactica su estadia en la aplicación que realmente es algo facil de hacer, siento que el codigo inicial quedo de una mejor manera .
