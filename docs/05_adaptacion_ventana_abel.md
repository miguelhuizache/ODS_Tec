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

