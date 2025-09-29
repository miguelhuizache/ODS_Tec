Resumen de la Sesión 2 (Problema y Éxito)

Problema Central: Los niños y adolescentes mexicanos carecen de herramientas digitales atractivas e interactivas que promuevan la toma de decisiones informadas sobre nutrición y actividad física diaria, resultando en altas tasas de obesidad. Población Objetivo: Menores de 18 años en México.

Criterios de Éxito (Mínimos del Prototipo):

    Registro de Datos Básico (Tasa de éxito ≥95%).

    Visualización de Progreso (Mostrar una gráfica semanal).

    Persistencia de Información (Guardar y recuperar datos de un archivo local).

    Usabilidad Mínima (Completar tarea de registro en <30 segundos).

------------------------------------------------------------------------------------------------------------------------

Lluvia de Ideas Generadas

    App de "Héroe Nutricional": Un tracker simple gamificado donde los usuarios "alimentan" a un avatar virtual con sus propios datos de comidas.

    Detector de Alimentos y Recetas Inteligente: App que analiza fotos de platos o alacenas para dar retroalimentación nutricional, sugerencias de mejora y recetas con los ingredientes disponibles.

        Calculadora de Déficit Calórico: Una herramienta centrada en el backend que calcula las calorías necesarias basándose en el registro de actividad y edad.

        Diario Visual de Hábitos: Una interfaz muy amigable con íconos de emojis que permite arrastrar y soltar los hábitos (e.g., "tomé agua", "jugué fútbol").

        Módulo de Retos Diarios: Propone pequeños retos de actividad física (e.g., "10 min de saltos") y verifica su cumplimiento con un simple checkbox.

------------------------------------------------------------------------------------------------------------------------

dea Elegida y Justificación

### Idea Base Seleccionada
**App de Nutrición Juvenil con Mascota Virtual: "NutriPet"**

### Descripción General
**NutriPet** es una aplicación **gamificada** que combina el cuidado de la salud con una **mascota virtual** que evoluciona en función de los **hábitos alimenticios saludables** del usuario. Su objetivo principal es promover una alimentación balanceada entre **adolescentes y jóvenes adultos** a través de la educación nutricional, la motivación y la interacción lúdica.

### Justificación
Esta idea fue seleccionada por ser la que mejor se alinea con la **motivación de la población juvenil** (gamificación y cuidado de una entidad digital). Ataca directamente la **falta de conciencia y motivación** al traducir el acto de comer sano en un beneficio tangible (la salud de la mascota), lo cual es crucial para la **adhesión a largo plazo**. Además, su enfoque modular permite cumplir con los **Criterios de Éxito** de registro de datos y visualización de progreso en la fase de Prototipo.

---

## Clasificación de Funcionalidades (MoSCoW)

| Categoría | Funcionalidad | Módulo Asociado (Arquitectura) |
| :--- | :--- | :--- |
| **MUST HAVE** | **Registro Básico de Comidas:** Entrada manual de una comida al día y calificación simple de "Sano/No Sano". | `app/`, `core/` |
| **MUST HAVE** | **Estado de la Mascota (Feedback Visual):** El estado de la mascota (ícono o mensaje) debe cambiar visiblemente si el usuario registra un día sano o no sano. | `app/`, `viz/` |
| **MUST HAVE** | **Persistencia:** Cargar y guardar el estado de la mascota y el historial de registros en un archivo local (JSON/TXT). | `data/` |
| **SHOULD HAVE** | **Métricas Básicas:** Mostrar un resumen de los días saludables completados durante la última semana. | `viz/` |
| **COULD HAVE** | **Evolución Simple:** Desbloqueo de un accesorio básico para la mascota después de 7 días saludables consecutivos. | `core/` |
| **WON'T HAVE** | **Análisis de Componentes con IA:** Escaneo de alimentos o detección avanzada de imágenes. | N/A |
| **WON'T HAVE** | **Conexión a APIs Médicas o Nutricionales Externas** | N/A |

---

## Wireflow Básico y Arquitectura Mínima (MVP)

### Wireflow (Flujo de Pantallas Básico)
1.  **Inicio/Creación de Mascota:** Elegir nombre/tipo de mascota y cargar datos previos.
2.  **Dashboard (Principal):** Vista de la Mascota (estado actual) y botones de acción ("Registrar Comida", "Ver Progreso").
3.  **Registro de Comida:** Ingreso de comida y calificación (Sano/No Sano).
4.  **Progreso Semanal:** Gráfica o resumen de días saludables completados.

### Arquitectura Mínima (Módulos)
| Módulo/Carpeta | Rol Principal | Descripción de Funcionalidad |
| :--- | :--- | :--- |
| **`app/`** | **Interfaz de Usuario (UI)** | Contiene el código principal de PySimpleGUI. Gestiona ventanas, botones y la interacción directa del usuario. |
| **`core/`** | **Lógica del Juego/Negocio** | El cerebro de **NutriPet**. Contiene la lógica que define el estado de la mascota, la validación de los registros y el cálculo de la recompensa. |
| **`data/`** | **Manejo de Archivos** | Responsable de la persistencia. Funciones para `guardar_estado_mascota()` y `cargar_historial()` desde archivos locales. |
| **`viz/`** | **Visualización/Feedback** | Contiene el código para mostrar la mascota (texto o imágenes dinámicas) y las métricas de progreso semanal. |
