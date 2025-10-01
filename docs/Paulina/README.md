

**Nombre:** Adriana Paulina Martínez Hernández  
**Matrícula:** A00575261  
**Ventana asignada:** Ventana 1 — *win_home.py* (ventana de bienvenida)

---

## Cambios aplicados
- La ventana funciona como **pantalla de bienvenida** del proyecto.  
- Se colocó un mensaje inicial claro y amigable.  
- Se añadió un botón que despliega un **MessageBox** para confirmar que el equipo está listo.  
- Se mantiene un botón **Cerrar** para salir de esta ventana fácilmente.  
- Se usó `ttk.Frame` y `ttk.Label` para mantener consistencia con las demás ventanas.  

---

## Captura de pantalla
Agrega aquí la captura real de la ventana al ejecutarla:

![Ventana de Bienvenida](./img/ventana_home_adriana.png)

---

## Reflexión
Esta ventana es importante porque da una primera impresión clara y profesional al usuario, con un mensaje de bienvenida y la opción de mostrar un aviso, ayuda a que la aplicación tenga un inicio **más bonito y accesible**, reforzando la idea del proyecto como un producto pensado para personas reales y no solo como un proyecto o tarea simplemente.  

---

## Código completo de la ventana (`src/app/win_home.py`)
```python
import tkinter as tk
from tkinter import ttk, messagebox

def open_win_home(parent: tk.Tk):
    win = tk.Toplevel(parent)
    win.title("Home / Bienvenida")
    win.geometry("360x220")

    frm = ttk.Frame(win, padding=16)
    frm.pack(fill="both", expand=True)

    ttk.Label(frm, text="¡Bienvenid@s!", font=("Segoe UI", 11, "bold")).pack(pady=(0, 8))
    ttk.Label(frm, text="Explora las ventanas desde la pantalla principal.").pack(pady=(0, 12))

    ttk.Button(frm, text="Mostrar mensaje",
               command=lambda: messagebox.showinfo("Info", "¡Equipo listo!")).pack()
    ttk.Button(frm, text="Cerrar", command=win.destroy).pack(pady=8)
