# Miguel Huizache Vázquez

## ¿Qué es esta ventana?
Es la **Ventana 5 (Canvas)**. Básicamente, un lienzo donde dibujamos cosas y jugamos con una mini animación. La idea es que no se vea “de tarea”, sino como una pantallita agradable y usable.

---

## ¿Qué le cambié para que se viera bonita?
- **Barra de acciones arriba** con botones claros: *Dibujar demo*, *Limpiar*, *Animar*, *Guardar .ps* y *Cerrar*.
- **Escena demo** con colores suaves: cielo, colinitas, sol, lago y un arbolito. Se ve amigable y bonita al abrir.
- **Grid sutil** (líneas muy claras) para darle “orden visual” sin distraer.
- **Pelotita animada** que rebota (play/pausa con un botón).
- **Barra de estado** abajo que muestra las coordenadas del mouse (útil para ubicarse en el lienzo).
- Todo con **tkinter/ttk** (sin librerías extra), para que corra en cualquier compu del equipo.

---

## ¿Cómo se usa?
1. **Dibujar demo:** arma la escena base (cielo, colinas, sol, lago, árbol) y coloca la pelotita.
2. **Animar:** la pelotita se mueve y rebota. Vuelve a presionar para pausar.
3. **Limpiar:** borra todo el canvas si quieres empezar de cero.
4. **Guardar .ps:** exporta el lienzo en formato PostScript (vectorial). Sirve para meterlo a un reporte sin que se pixelee.
5. **Cerrar:** cierra la ventana.
6. **Tip:** mueve el mouse sobre el canvas; abajo verás las coordenadas en tiempo real.

---

## ¿Por qué estos cambios ayudan al proyecto?
- **Mejor primera impresión:** el usuario abre esta ventana y ya ve una escena linda. Se siente “producto”, no solo código.
- **Demuestra capacidades del Canvas:** formas, colores, capas, animación sencilla, interacción con el mouse.
- **Más claridad:** la barra de acciones arriba y la barra de estado abajo hacen que todo sea obvio de usar.
- **Estandariza el look & feel:** se alinea con el resto de ventanas (ttk, márgenes, botones con intención).

---

## Captura de pantalla



---

## Resumen técnico rápido
- `Canvas` con fondo blanco y borde sutil.
- Escena dibujada con `create_rectangle`, `create_oval`, `create_line`, `create_text`.
- Animación con `after(16, ...)` (~60 FPS), controlada por un flag `anim_running`.
- Redibujo básico al redimensionar: se reconstruye la escena para que el grid no se vea cortado.
- Exportación con `canvas.postscript(..., colormode="color")`.

---

## Posibles mejoras (si queremos subirlo de nivel)
- Exportar **PNG** (requiere `Pillow`).
- Slider para controlar **velocidad** de la pelotita.
- Herramientas de **dibujo libre** (lápiz, rectángulo, círculo).
- “Temas” de color (claro/oscuro).
- Snap al grid (para colocar objetos exactos).


