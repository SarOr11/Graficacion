### Generación de Polígonos Regulares mediante Scripting (bpy).

Este proyecto demuestra el uso de la API de Python en Blender (bpy) para la creación automatizada de geometrías 2D. Se utiliza la conversión de coordenadas polares a cartesianas para posicionar vértices y generar polígonos regulares de _n_ lados.

### 1. Configuración del entorno de Blender
Antes de ejecutar el script, debemos configurar el entorno para trabajar en Blender por medio de los siguientes pasos:

1. Abrir Blender: Inicia una nueva instancia de Blender (versión 3.0 o superior).

<img width="1918" height="1017" alt="Image" src="https://github.com/user-attachments/assets/95f712ba-1c66-4f42-976e-ec99ea563328" />


2. Da click en la pestaña _Scripts_ en la barra superior del menú.

<img width="1918" height="1015" alt="Image" src="https://github.com/user-attachments/assets/2b190317-210a-4366-9e11-71c51d0b05fe" />


3. Selecciona _Nuevo_ para crear un nuevo bloque de texto.

<img width="1918" height="1018" alt="Image" src="https://github.com/user-attachments/assets/3398e2d2-992a-44c0-81d1-2450a59ac53c" />

### 2. Implementación
El script se basa en la división de una circunferencia de _2\pi_ radianes entre el número de lados deseados (n):

```
import bpy
import math

def crear_poligono_2d(nombre, lados, radio):
    # Crear una nueva malla y un nuevo objeto
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)
    
    #Vincular el objeto a la escena actual
    bpy.context.collection.objects.link(objeto)
    
    vertices = []
    aristas =[]
    
    #Cálculo de vertices usando coordenadas polares a cartesianas
    for i in range(lados):
        angulo = 2*math.pi*i/lados
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)
        vertices.append((x, y, 0))
        
    #Definir las aristas entre los vértices   
    for i in range(lados):
        aristas.append((i, (i + 1) % lados))
        
    #Cargar los datos en la malla 
    malla.from_pydata(vertices, aristas, [])
    malla.update()
    
#Limpiar la escena antes de empezar    
bpy.ops.object.select_all(action = 'SELECT')
bpy.ops.object.delete()

#Llamada a la función   
crear_poligono_2d("Poligono2D", lados=6, radio=5)
```

**Explicación**
- `bpy.data.meshes.new():` Crea la estructura de datos para la malla.
- `malla.from_pydata()`: Es el corazón del script. Toma tres listas: vértices, aristas (edges) y caras (faces). En esta práctica, solo usamos aristas para generar un "wireframe".
- `(%)`: Se utiliza en (i + 1) % lados para asegurar que el último vértice se conecte automáticamente con el primero, cerrando el polígono.

### 3. Ejecución 
Para ejecutar el código:

1. Copia el script en el editor de texto de Blender.

<img width="1918" height="1022" alt="Image" src="https://github.com/user-attachments/assets/b9f4d6cf-1655-4334-9460-8f32cf97dfd7" />


2. Presiona _Run Script_ o Alt+P.

<img width="1918" height="1022" alt="Image" src="https://github.com/user-attachments/assets/7bda1533-0c54-4570-b68b-369ab9f3db7f" />


3. El script limpiará la escena automáticamente y generará el polígono configurado.

<img width="1920" height="1015" alt="Image" src="https://github.com/user-attachments/assets/a0b5393f-1197-4678-ab4f-74ee8c4a7bf6" />

### Conclusiones
El uso de scripts para la generación de primitivas permite un control total sobre el objeto. Esta técnica es fundamental para el desarrollo de herramientas de modelado procedural y visualización de datos complejos en 3D.
