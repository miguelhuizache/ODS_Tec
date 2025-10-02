# Gráficas modificadas sesión 7

## 📊 Visualización

![Gráficas de sesión 7](ruta/a/tu-imagen.png) <!-- Cambia esta ruta por la real -->

## 🐍 Código en Python

```python
# Aquí va el código Python
# Por ejemplo:
import matplotlib.pyplot as plt

# Este es solo un ejemplo básico
horas = ['01:00', '02:00', '03:00']
temperatura = [18, 19, 21]

plt.plot(horas, temperatura, color='red', marker='o')
plt.title("Temperatura - Ciudad de México")
plt.xlabel("Hora")
plt.ylabel("°C")
plt.grid(True)
plt.show()
