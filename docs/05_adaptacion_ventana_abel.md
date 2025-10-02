# 05 – Adaptación de la ventana al proyecto (NutriTec)

## 1) Contexto
Se adaptó la **ventana de tabla** (`app/win_table.py`) para el proyecto **NutriTec** (ODS 3: Salud y bienestar), enfocada en el **registro diario de hábitos** usando un **semáforo** (Verde/Amarillo/Rojo), minutos de actividad, puntos y glucosa opcional.

## 2) Objetivo de la ventana
- Capturar, consultar y editar registros diarios.
- Mostrar tabla con ordenamiento y filtro por columna.
- Exportar los datos a CSV.

## 3) Esquema de datos (columnas)
| Campo        | Tipo        | Descripción                                         |
|--------------|-------------|-----------------------------------------------------|
| ID           | entero      | Identificador autoincremental.                      |
| Fecha        | texto       | Formato `YYYY-MM-DD`.                               |
| Comida       | texto       | Descripción breve de la comida/hábito.             |
| Semaforo     | texto       | `Verde`, `Amarillo` o `Rojo`.                       |
| MinActividad | entero      | Minutos de actividad física.                        |
| Puntos       | entero      | Monedas/puntos ganados.                             |
| Glucosa      | entero/vacío| Valor opcional si aplica.                           |

> En el código:
> `COLUMNAS = ("ID","Fecha","Comida","Semaforo","MinActividad","Puntos","Glucosa")`

## 4) Reglas de negocio (MVP)
- El usuario selecciona el color del **Semáforo** al registrar.
- Sugerencia de puntos: **Verde = +5**, **Amarillo = +2**, **Rojo = +0**.
- `MinActividad`, `Puntos` y `Glucosa` aceptan solo enteros (Glucosa puede ir vacío).

## 5) Integración con la app
En `main.py`:
```python
from app.win_table import open_win_table
# …
ttk.Button(frame, text="Tabla / Hábitos", command=lambda: open_win_table(root)).pack(pady=6)

