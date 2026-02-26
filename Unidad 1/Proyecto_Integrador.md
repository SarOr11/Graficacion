# Proyecto integrador: Construcción procedural de un escenario 3D evolucionado.

## 1. Objetivos del proyecto

Implementar un entorno tridimensional dinámico mediante scripting en Blender (Python), integrando conceptos fundamentales como:
- Transformaciones tridimensionales (Traslación, Rotación y Escalamiento).
- Modelos de color (RGB) y materiales avanzados.
- Geometría curva mediante funciones trigonométricas.

## 2. Fundamentos teóricos aplicados

El proyecto se basa en los siguientes pilares de la graficación:
- Modelos de Color: Uso del modelo RGB para la creación de materiales difusos y nodos de sombreado (Principled BSDF).
- Transformaciones Lineales: Traslación: Posicionamiento de bloques en los ejes X, Y, Z.
- Escalamiento: Ajuste de dimensiones para el suelo y variaciones en la altura de las paredes z=1.5.
- Rotación: Orientación de los bloques en el tramo curvo para mantener la tangencia a la trayectoria.
- Lógica Procedural: Uso de ciclos for y condicionales para la generación automática de geometría y alternancia de materiales.

## 3. Desarrollo de la práctica

### 3.1. Preparación y gestión de memoria

Se implementó una rutina de limpieza inicial que elimina cualquier objeto previo en la escena, asegurando una gestión eficiente de la memoria y un entorno de trabajo limpio en cada ejecución del script.

### 3.2. Creación de materiales con nodos
A diferencia del modelo básico inicial, se evolucionó la función crear_material para utilizar el sistema de nodos de Blender, permitiendo un control más preciso sobre propiedades como la rugosidad (Roughness) y la salida del sombreador Principled BSDF.

### 3.3. Construcción del escenario (recto a curvo)
El escenario se divide en dos secciones principales:

### Pasillo recto
Generado mediante una iteración lineal en el eje Y, alternando materiales según el índice del ciclo.

<img width="1918" height="1016" alt="Image" src="https://github.com/user-attachments/assets/79d9da7b-0b56-43c1-81b2-09844ed18da5" />

<img width="1915" height="987" alt="Image" src="https://github.com/user-attachments/assets/d130fba5-d91e-48fa-a343-4fba0e6df6ad" />


### Transición curva
Se implementó lógica matemática utilizando math.sin() y math.cos() para calcular las coordenadas $(x, y)$ de los bloques en un arco de 90 grados ($\pi/2$).Se aplicó una rotación en el eje Z calculada dinámicamente para que las paredes sigan la curvatura del pasillo de forma natural.

<img width="1912" height="1018" alt="Image" src="https://github.com/user-attachments/assets/5cbd17a1-d2bf-444f-aaea-2b0e742a6794" />

<img width="1917" height="1015" alt="Image" src="https://github.com/user-attachments/assets/9153deb5-aa50-4a09-b642-c39a4333cdb0" />


### 3.4. Implementación de cámara y animación

Para transformar el escenario en una experiencia de "videojuego", se añadió:
- Cámara en primera persona: Ubicada a una altura de $1.5$ unidades.
- Trayectoria (Path): Una curva invisible que dicta el recorrido de la cámara desde el inicio del pasillo recto hasta el final de la curva.
- Restricción Follow Path: La cámara y un objeto "Target" se desplazan por la curva mediante keyframes (fotogramas clave), animando el factor de desplazamiento (offset_factor) del frame 1 al 200.

<img width="1918" height="1017" alt="Image" src="https://github.com/user-attachments/assets/d7a29353-416d-45db-a2dc-67d375134a01" />

<img width="1918" height="1017" alt="Image" src="https://github.com/user-attachments/assets/eac54414-63d5-4ca4-97f9-231d5f438785" />

## 4. Resultados
Se obtuvo un escenario 3D generado íntegramente por código, que presenta una transición fluida entre una estructura lineal y una curva.

- Paredes: Alternan entre un gris oscuro base y un naranja rojizo de acento.
- Suelo: Un plano escalado que unifica ambas secciones del recorrido.
- Iluminación: Se integró una luz tipo "Sun" para iluminación global y una luz de punto ("Point") para resaltar la profundidad del pasillo.

<img width="1917" height="1017" alt="Image" src="https://github.com/user-attachments/assets/516b22d5-4d3d-42b6-a9ba-49bb1a4a68ef" />

El recorrido de la cámara se representa a continuación en los siguientes frames:

<img width="1913" height="1016" alt="Image" src="https://github.com/user-attachments/assets/95920835-9fcb-44be-9498-5a55ee5218f3" />

<img width="1915" height="1013" alt="Image" src="https://github.com/user-attachments/assets/4eeacc04-8d1f-4d27-8fc9-793f703f81f7" />

<img width="1917" height="1012" alt="Image" src="https://github.com/user-attachments/assets/1f90a223-d75c-4a11-a964-efddc3f773d6" />

## 5. Conclusiones

La práctica del proyecto integrador, además de  introducir los pilares fundamentales de la graficación por computadora, demuestra la potencia del diseño procedural. La capacidad de transformar un pasillo recto en uno curvo mediante ajustes trigonométricos evidencia que las matemáticas son la herramienta principal del desarrollador de gráficos. La inclusión de una cámara animada eleva el proyecto de una simple visualización estática a una base funcional para un motor de videojuegos o un recorrido arquitectónico virtual, lo cual añade complejidad en proyectos pertenecientes a dichos sectores.


## Anexo: Código completo

```
import bpy
import math

def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.use_nodes = True
    nodes = mat.node_tree.nodes
    
    nodes.clear()
    

    node_principled = nodes.new(type='ShaderNodeBsdfPrincipled')
    node_output = nodes.new(type='ShaderNodeOutputMaterial')
    
    links = mat.node_tree.links
    links.new(node_principled.outputs['BSDF'], node_output.inputs['Surface'])
    

    node_principled.inputs[0].default_value = (*color_rgb, 1.0) 
    # 'Roughness' suele ser la entrada 2 o 9 según versión, mejor usar el nombre interno
    if 'Roughness' in node_principled.inputs:
        node_principled.inputs['Roughness'].default_value = 0.7
        
    return mat

def generar_escenario():

    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()


    mat_pared_a = crear_material("ParedOscura", (0.1, 0.1, 0.1))
    mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0))


    largo_pasillo = 10
    ancho_pasillo = 4
    radio_curva = 12 


    for i in range(largo_pasillo):
        bpy.ops.mesh.primitive_cube_add(location=(-ancho_pasillo, i * 2, 1))
        pared_izq = bpy.context.active_object
        if i % 2 == 0:
            pared_izq.data.materials.append(mat_pared_a)
        else:
            pared_izq.data.materials.append(mat_pared_b)
            pared_izq.scale.z = 1.5

        bpy.ops.mesh.primitive_cube_add(location=(ancho_pasillo, i * 2, 1))
        pared_der = bpy.context.active_object
        pared_der.data.materials.append(mat_pared_a)


    cy = (largo_pasillo - 1) * 2 
    cx = radio_curva 

    for j in range(1, largo_pasillo + 1):
        angulo = math.pi - (j * (math.pi / 2) / largo_pasillo)
        rotacion_z = math.pi - angulo 

  
        x_izq = cx + (radio_curva + ancho_pasillo) * math.cos(angulo)
        y_izq = cy + (radio_curva + ancho_pasillo) * math.sin(angulo)
        bpy.ops.mesh.primitive_cube_add(location=(x_izq, y_izq, 1), rotation=(0, 0, rotacion_z))
        pared_izq_curva = bpy.context.active_object
        
 
        if (largo_pasillo + j) % 2 == 0:
            pared_izq_curva.data.materials.append(mat_pared_a)
        else:
            pared_izq_curva.data.materials.append(mat_pared_b)
            pared_izq_curva.scale.z = 1.5


        x_der = cx + (radio_curva - ancho_pasillo) * math.cos(angulo)
        y_der = cy + (radio_curva - ancho_pasillo) * math.sin(angulo)
        bpy.ops.mesh.primitive_cube_add(location=(x_der, y_der, 1), rotation=(0, 0, rotacion_z))
        pared_der_curva = bpy.context.active_object
        pared_der_curva.data.materials.append(mat_pared_a)


    bpy.ops.mesh.primitive_plane_add(size=1, location=(radio_curva/2, cy/2 + radio_curva/2, 0))
    suelo = bpy.context.active_object
    suelo.scale.x = (ancho_pasillo * 2) + radio_curva + 10
    suelo.scale.y = (largo_pasillo * 2) + radio_curva + 10

  
    bpy.ops.object.light_add(type='SUN', location=(10, 10, 10), rotation=(math.radians(-45), math.radians(30), 0))
    sun = bpy.context.active_object
    sun.data.energy = 3 

    bpy.ops.object.light_add(type='POINT', location=(0, 5, 4))
    luz1 = bpy.context.active_object
    luz1.data.energy = 500 

    # Camara
    bpy.ops.object.camera_add(location=(0, 0, 0), rotation=(0, 0, 0))
    camera = bpy.context.active_object
    bpy.context.scene.camera = camera

    #Camino
    curve_data = bpy.data.curves.new('CamPathData', type='CURVE')
    curve_data.dimensions = '3D'
    curve_data.use_path = True
    spline = curve_data.splines.new('POLY') 
    
    puntos_camino = [(0, -6, 1.5), (0, cy, 1.5)]
    pasos_curva = 20
    for step in range(1, pasos_curva + 1):
        progreso = step / float(pasos_curva)
        ang = math.pi - (progreso * (math.pi / 2))
        px = cx + radio_curva * math.cos(ang)
        py = cy + radio_curva * math.sin(ang)
        puntos_camino.append((px, py, 1.5))

    spline.points.add(len(puntos_camino) - 1)
    for idx, pt in enumerate(puntos_camino):
        spline.points[idx].co = (*pt, 1.0) 

    cam_path = bpy.data.objects.new('Cam_Path', curve_data)
    bpy.context.scene.collection.objects.link(cam_path)

    # 9. Restricciones
    bpy.ops.object.empty_add(type='PLAIN_AXES', location=(0, 0, 0))
    cam_target = bpy.context.active_object
    cam_target.name = "Cam_Target"

    fp_target = cam_target.constraints.new(type='FOLLOW_PATH')
    fp_target.target = cam_path
    fp_target.use_fixed_location = True

    follow_path = camera.constraints.new(type='FOLLOW_PATH')
    follow_path.target = cam_path
    follow_path.use_fixed_location = True 

    track_to = camera.constraints.new(type='TRACK_TO')
    track_to.target = cam_target
    track_to.track_axis = 'TRACK_NEGATIVE_Z'
    track_to.up_axis = 'UP_Y'

    #Animación
    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = 200

    follow_path.offset_factor = 0.0
    fp_target.offset_factor = 0.05 
    camera.keyframe_insert(data_path=f'constraints["{follow_path.name}"].offset_factor', frame=1)
    cam_target.keyframe_insert(data_path=f'constraints["{fp_target.name}"].offset_factor', frame=1)

    follow_path.offset_factor = 0.95 
    fp_target.offset_factor = 1.0 
    camera.keyframe_insert(data_path=f'constraints["{follow_path.name}"].offset_factor', frame=200)
    cam_target.keyframe_insert(data_path=f'constraints["{fp_target.name}"].offset_factor', frame=200)

    bpy.context.view_layer.update()

generar_escenario()

```
