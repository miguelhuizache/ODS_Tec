Ventana asignada: Ventana 3 — win_list.py 

¿Qué cambié?
Pasé de una lista sencilla a un formulario interactivo orientado a salud/alimentación.
Agregué campos clave:
Diabetes (sí/no) y, si es “sí”, un tipo de diabetes con Combobox.
Peso (kg), altura (cm) y comidas por día.
¿Sabes tus calorías diarias? (sí/no) + campo para capturarlas si responde “sí”.
¿Haces ejercicio? (sí/no) + tipo y frecuencia si responde “sí”.
Hice los campos más interactivos: se muestran/ocultan según las respuestas.
Botón Guardar que consolida todo en un diccionario y lo muestra en un messagebox.
UI con ttk, ordenada por filas/columnas para que sea fácil de leer y editar.

Imagen de la propuesta:
https://github.com/user-attachments/assets/df79690f-6516-4a18-8ffa-61e4a6b3b962
[win_listdesas.py](https://github.com/user-attachments/files/22611140/win_listdesas.py)

¿Por qué estos cambios ayudan al proyecto?

Porque nos acercan a un caso real, no solo “guardamos datos”, sino que entendemos al usuario (estado de salud, hábitos y actividad). 
Con esto, más adelante podemos generar recomendaciones o métricas útiles. Además, la UI dinámica evita cosas: solo mostramos lo que importa según cada respuesta y 
es más limpio, usable y alineado a una app de bienestar.

Código nuevo:

[Uploading win_listdesas.py…]()
import tkinter as tk
from tkinter import ttk, messagebox

def open_win_list(parent: tk.Tk):
    win = tk.Toplevel(parent)
    win.title("Formulario de Datos Personales")
    win.geometry("500x500")

    frm = ttk.Frame(win, padding=16)
    frm.pack(fill="both", expand=True)

    # Diabetes
    ttk.Label(frm, text="¿Tienes diabetes?").grid(row=0, column=0, sticky="w")
    diabetes_var = tk.StringVar(value="no")
    diabetes_si = ttk.Radiobutton(frm, text="Sí", variable=diabetes_var, value="si")
    diabetes_no = ttk.Radiobutton(frm, text="No", variable=diabetes_var, value="no")
    diabetes_si.grid(row=0, column=1, sticky="w")
    diabetes_no.grid(row=0, column=2, sticky="w")

    # Tipo de diabetes
    tipo_diabetes_label = ttk.Label(frm, text="¿Qué tipo de diabetes tienes?")
    tipo_diabetes_var = tk.StringVar()
    tipo_diabetes_combo = ttk.Combobox(frm, textvariable=tipo_diabetes_var, values=["Tipo 1", "Tipo 2", "Gestacional", "Otro"])

    # Peso
    ttk.Label(frm, text="¿Cuál es tu peso? (kg)").grid(row=2, column=0, sticky="w")
    peso_var = tk.StringVar()
    peso_entry = ttk.Entry(frm, textvariable=peso_var)
    peso_entry.grid(row=2, column=1, columnspan=2, sticky="ew")

    # Altura
    ttk.Label(frm, text="¿Cuál es tu altura? (cm)").grid(row=3, column=0, sticky="w")
    altura_var = tk.StringVar()
    altura_entry = ttk.Entry(frm, textvariable=altura_var)
    altura_entry.grid(row=3, column=1, columnspan=2, sticky="ew")

    # Veces que come al día
    ttk.Label(frm, text="¿Cuántas veces comes al día?").grid(row=4, column=0, sticky="w")
    comidas_var = tk.StringVar()
    comidas_entry = ttk.Entry(frm, textvariable=comidas_var)
    comidas_entry.grid(row=4, column=1, columnspan=2, sticky="ew")

    # Sabe calorías
    ttk.Label(frm, text="¿Sabes cuántas calorías consumes diariamente?").grid(row=5, column=0, sticky="w")
    calorias_sabe_var = tk.StringVar(value="no")
    calorias_si = ttk.Radiobutton(frm, text="Sí", variable=calorias_sabe_var, value="si")
    calorias_no = ttk.Radiobutton(frm, text="No", variable=calorias_sabe_var, value="no")
    calorias_si.grid(row=5, column=1, sticky="w")
    calorias_no.grid(row=5, column=2, sticky="w")

    # Cantidad de calorías
    calorias_label = ttk.Label(frm, text="Cantidad total de calorías diarias:")
    calorias_var = tk.StringVar()
    calorias_entry = ttk.Entry(frm, textvariable=calorias_var)

    # Ejercicio
    ttk.Label(frm, text="¿Haces ejercicio regularmente?").grid(row=7, column=0, sticky="w")
    ejercicio_var = tk.StringVar(value="no")
    ejercicio_si = ttk.Radiobutton(frm, text="Sí", variable=ejercicio_var, value="si")
    ejercicio_no = ttk.Radiobutton(frm, text="No", variable=ejercicio_var, value="no")
    ejercicio_si.grid(row=7, column=1, sticky="w")
    ejercicio_no.grid(row=7, column=2, sticky="w")

    # Tipo de ejercicio
    tipo_ejercicio_label = ttk.Label(frm, text="¿Qué tipo de ejercicio haces?")
    tipo_ejercicio_var = tk.StringVar()
    tipo_ejercicio_entry = ttk.Entry(frm, textvariable=tipo_ejercicio_var)

    # Frecuencia de ejercicio
    frecuencia_ejercicio_label = ttk.Label(frm, text="¿Cada cuánto haces ejercicio?")
    frecuencia_ejercicio_var = tk.StringVar()
    frecuencia_ejercicio_entry = ttk.Entry(frm, textvariable=frecuencia_ejercicio_var)

    # Funciones para mostrar/ocultar campos
    def actualizar_campos(*args):
        # Diabetes
        if diabetes_var.get() == "si":
            tipo_diabetes_label.grid(row=1, column=0, sticky="w")
            tipo_diabetes_combo.grid(row=1, column=1, columnspan=2, sticky="ew")
        else:
            tipo_diabetes_label.grid_remove()
            tipo_diabetes_combo.grid_remove()
        # Calorías
        if calorias_sabe_var.get() == "si":
            calorias_label.grid(row=6, column=0, sticky="w")
            calorias_entry.grid(row=6, column=1, columnspan=2, sticky="ew")
        else:
            calorias_label.grid_remove()
            calorias_entry.grid_remove()
        # Ejercicio
        if ejercicio_var.get() == "si":
            tipo_ejercicio_label.grid(row=8, column=0, sticky="w")
            tipo_ejercicio_entry.grid(row=8, column=1, columnspan=2, sticky="ew")
            frecuencia_ejercicio_label.grid(row=9, column=0, sticky="w")
            frecuencia_ejercicio_entry.grid(row=9, column=1, columnspan=2, sticky="ew")
        else:
            tipo_ejercicio_label.grid_remove()
            tipo_ejercicio_entry.grid_remove()
            frecuencia_ejercicio_label.grid_remove()
            frecuencia_ejercicio_entry.grid_remove()

    diabetes_var.trace_add("write", actualizar_campos)
    calorias_sabe_var.trace_add("write", actualizar_campos)
    ejercicio_var.trace_add("write", actualizar_campos)
    actualizar_campos()

    def guardar():
        datos = {
            "diabetes": diabetes_var.get(),
            "tipo_diabetes": tipo_diabetes_var.get() if diabetes_var.get() == "si" else None,
            "peso": peso_var.get(),
            "altura": altura_var.get(),
            "comidas": comidas_var.get(),
            "sabe_calorias": calorias_sabe_var.get(),
            "calorias": calorias_var.get() if calorias_sabe_var.get() == "si" else None,
            "ejercicio": ejercicio_var.get(),
            "tipo_ejercicio": tipo_ejercicio_var.get() if ejercicio_var.get() == "si" else None,
            "frecuencia_ejercicio": frecuencia_ejercicio_var.get() if ejercicio_var.get() == "si" else None,
        }
        messagebox.showinfo("Datos guardados", f"Datos ingresados:\n{datos}")

    ttk.Button(frm, text="Guardar", command=guardar).grid(row=10, column=0, columnspan=2, pady=12)
    ttk.Button(frm, text="Cerrar", command=win.destroy).grid(row=11, column=0, columnspan=2, pady=6)

  
