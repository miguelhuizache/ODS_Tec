## Resumen de la Sesión 2 (Problema y Éxito)

* **Problema Central:** Los niños y adolescentes mexicanos carecen de herramientas digitales atractivas e interactivas que promuevan la toma de decisiones informadas sobre nutrición y actividad física diaria, resultando en altas tasas de obesidad.
* **Población Objetivo:** Menores de 18 años en México.

### Criterios de Éxito (Mínimos del Prototipo)
* **Registro de Datos Básico** (Tasa de éxito $\ge 95\%$).
* **Visualización de Progreso** (Mostrar una gráfica semanal).
* **Persistencia de Información** (Guardar y recuperar datos de un archivo local).
* **Usabilidad Mínima** (Completar tarea de registro en $< 30$ segundos).

---

## Lluvia de Ideas Generadas

| Idea de Solución | Descripción Breve |
| :--- | :--- |
| **App de "Héroe Nutricional"** | Un *tracker* simple gamificado donde los usuarios "alimentan" a un avatar virtual con sus propios datos de comidas. |
| **Detector de Alimentos y Recetas Inteligente** | App que analiza fotos de platos o alacenas para dar retroalimentación nutricional, sugerencias de mejora y recetas con los ingredientes disponibles. |
| **Calculadora de Déficit Calórico** | Una herramienta centrada en el *backend* que calcula las calorías necesarias basándose en el registro de actividad y edad. |
| **Diario Visual de Hábitos** | Una interfaz muy amigable con íconos de *emojis* que permite arrastrar y soltar los hábitos (e.g., "tomé agua", "jugué fútbol"). |
| **Módulo de Retos Diarios** | Propone pequeños retos de actividad física (e.g., "10 min de saltos") y verifica su cumplimiento con un simple *checkbox*. |

---

### Idea Base Seleccionada
**Asistente Nutricional Personalizado con Gamificación: "NutriTec"**

### Descripción General
**NutriTec** es una aplicación dirigida a **adultos** que buscan mejorar sus hábitos o manejar condiciones metabólicas. Utiliza un **semáforo visual** para calificar comidas y un **sistema de puntos gamificado** para fomentar la toma de decisiones conscientes. Incluye una **mascota virtual** que refleja el progreso semanal del usuario y un **diario específico** para el seguimiento de parámetros clave (como glucosa, opcional).

### Características Principales (Reintegración de la Mascota)
* **Mascota/Avatar de Progreso:** La mascota virtual es el **reflejo del éxito del usuario**. Su apariencia o estado de ánimo dependerá del balance semanal de los colores del semáforo.
* **Tienda de Recompensas (Monedas):** El usuario gana **monedas virtuales** al obtener registros "Verdes", las cuales puede usar para comprar elementos cosméticos o mejoras para su mascota/avatar.

### Justificación
Esta idea fue seleccionada por ser la que mejor se adapta al nuevo enfoque de **adultos conscientes de su salud** y porque:

1.  **Enfoque Práctico y Decisorio:** Se centra en dar **retroalimentación inmediata** a través de un **semáforo** (Verde/Amarillo/Rojo) al adulto, facilitando la toma de decisiones conscientes.
2.  **Mascota como Motivador Adulto:** La **mascota virtual** actúa como un *recordatorio visual y lúdico* de las consecuencias de sus hábitos, apelando a la motivación del adulto para mantener la disciplina y ganar recompensas (monedas).
3.  **Cumple Criterios Técnicos:** Su enfoque en el **registro, feedback visual** y **persistencia de datos** es totalmente compatible con los requisitos mínimos del prototipo definidos en la Sesión 2.

---

## Clasificación de Funcionalidades (MoSCoW)

| Categoría | Funcionalidad | Módulo Asociado (Arquitectura) |
| :--- | :--- | :--- |
| **MUST HAVE** | **Registro Diario Básico:** Entrada manual de comidas. | `app/`, `core/` |
| **MUST HAVE** | **Semáforo Visual de Conciencia:** El usuario clasifica su comida registrada como **Verde/Amarillo/Rojo** y el sistema muestra un resumen de estos colores semanalmente. | `app/`, `core/`, `viz/` |
| **MUST HAVE** | **Mascota/Avatar de Progreso (Estado Mínimo):** La mascota debe cambiar su estado (e.g., feliz/neutral) o apariencia en función del balance del semáforo semanal. | `app/`, `core/`, `viz/` |
| **MUST HAVE** | **Persistencia:** Cargar y guardar el perfil del usuario, el contador de monedas y los registros en un archivo local (JSON/TXT). | `data/` |
| **SHOULD HAVE** | **Sistema de Monedas/Puntos Básico:** Otorgar puntos por cada registro "Verde" y permitir gastarlos en una mejora cosmética simple para la mascota. | `core/` |
| **COULD HAVE** | **Módulo Extra (Glucosa):** Campo de registro opcional para niveles de glucosa en sangre (si el perfil lo indica). | `core/` |
| **WON'T HAVE** | **Análisis de Fotos Complejo (IA):** Detección de alimentos en fotos de platos o alacena. | N/A |
| **WON'T HAVE** | **Conexión a APIs Médicas o Nutricionales Externas.** | N/A |

---

## Wireflow Básico y Arquitectura Mínima (MVP)

### Wireflow (Flujo de Pantallas Básico)
1.  **Inicio/Perfil:** Creación de perfil (edad, meta, ¿usuario con diabetes?).
2.  **Dashboard (Principal):** Muestra la **Mascota/Avatar** (estado actual), balance semanal (colores del semáforo), contador de monedas y botones de acción.
3.  **Registro Diario:** Pantalla con campos para: 1. Descripción de la comida. 2. Clasificación **Verde/Amarillo/Rojo**. 3. Campo de **Glucosa** (opcional).
4.  **Historial/Reporte:** Vista de los registros y resumen semanal.

### Arquitectura Mínima (Módulos)
| Módulo/Carpeta | Rol Principal | Descripción de Funcionalidad |
| :--- | :--- | :--- |
| **`app/`** | **Interfaz de Usuario (UI)** | Contiene el código principal de PySimpleGUI. Gestiona ventanas, la presentación de la **mascota** y la interacción directa del usuario. |
| **`core/`** | **Lógica de Negocio y Reglas** | El cerebro de **NutriTec**. Contiene la lógica para asignar puntos, calcular el balance del semáforo y validar la entrada de datos. También gestiona el estado de la **mascota**. |
| **`data/`** | **Manejo de Archivos** | Responsable de la persistencia. Funciones para `guardar_perfil()`, `cargar_registros()` y el estado de la **mascota**/**monedas** desde archivos locales (JSON/TXT). |
| **`viz/`** | **Visualización de Datos** | Contiene el código para mostrar el estado visual de la **mascota** (texto o íconos dinámicos) y el resumen semanal del semáforo. |
