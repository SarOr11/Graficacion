### Práctica 2: Generación de Patrones Geométricos (Flor de la Vida)

En esta práctica se explora la creación de patrones complejos mediante la repetición de primitivas geométricas. Utilizando el módulo math de Python, calculamos posiciones siguiendo una trayectoria circular para crear una estructura basada en el concepto de la "Flor de la Vida" o patrones radiales.

### 1. Preparación del entorno

Antes de ejecutar el script, debemos configurar el entorno para trabajar en Blender por medio de los siguientes pasos:

1. Abrir Blender: Inicia una nueva instancia de Blender (versión 3.0 o superior).

<img width="1918" height="1017" alt="Image" src="https://github.com/user-attachments/assets/95f712ba-1c66-4f42-976e-ec99ea563328" />


2. Da click en la pestaña _Scripts_ en la barra superior del menú.

<img width="1918" height="1015" alt="Image" src="https://github.com/user-attachments/assets/2b190317-210a-4366-9e11-71c51d0b05fe" />


3. Selecciona _Nuevo_ para crear un nuevo bloque de texto.

<img width="1918" height="1018" alt="Image" src="https://github.com/user-attachments/assets/3398e2d2-992a-44c0-81d1-2450a59ac53c" />

### 2. Implementación matemática y lógica
El código utiliza un ciclo `while` para distribuir círculos uniformemente en un radio determinado.
A diferencia de la práctica del polígono (donde se conectaban vértices), aquí cada iteración coloca un objeto completo `(primitive_circle_add) `en una coordenada calculada.

```
import bpy
import math

# Limpiar escena
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete(use_global=False)

# Configuración
radio = 2
angulo_actual = 0
numero_de_circulos = 20 
paso = 360 / numero_de_circulos  # Cálculo automático del paso (18 grados)

# Círculo central
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0, 0, 0))

while angulo_actual < 360:
    # Convertir grados a radianes
    theta = math.radians(angulo_actual)
    
    # Coordenadas polares -> cartesianas
    x = radio * math.cos(theta)
    y = radio * math.sin(theta)
    
    # Crear círculo en la nueva posición radial
    bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x, y, 0))
    
    # Actualizar el ángulo
    angulo_actual += paso

```
**Parámetros de control:**

- Radio: Define tanto el tamaño de los círculos individuales como la distancia desde el centro original.
- Paso (Ángulo): Determina la separación entre cada círculo.
   Por ejemplo, para 6 círculos el paso es de 60° (360/6). Para 20 círculos, el paso se reduce a 18° (360/20).


### 3. Resultados y comparativa

**Con 6 círculos (Paso 60°):**

<img width="597" height="401" alt="Image" src="https://github.com/user-attachments/assets/994ff38b-343f-424f-a521-7f6e4f52ab9b" />

**Con 20 círculos (Paso 18°):** 

<img width="599" height="407" alt="Image" src="https://github.com/user-attachments/assets/0c428763-6eba-449e-9523-016594bdac70" />

### 4. Conclusiones

Esta práctica demuestra cómo el Scripting Procedural permite generar arte generativo y estructuras simétricas que serian muy tediosas de colocar manualmente. Al automatizar el cálculo de x e y mediante funciones trigonométricas, podemos escalar la complejidad del diseño simplemente cambiando una variable.
