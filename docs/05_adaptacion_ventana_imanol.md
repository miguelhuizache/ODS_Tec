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

¿Por qué estos cambios ayudan al proyecto?

Porque nos acercan a un caso real, no solo “guardamos datos”, sino que entendemos al usuario (estado de salud, hábitos y actividad). 
Con esto, más adelante podemos generar recomendaciones o métricas útiles. Además, la UI dinámica evita cosas: solo mostramos lo que importa según cada respuesta y 
es más limpio, usable y alineado a una app de bienestar.

