<!-- ÍNDICE -->
<h2>Índice</h2>
<ul>
  <li><a href="#21-transformación-bidimensional">2.1 Transformación bidimensional</a>
    <ul>
      <li><a href="#211-traslación">2.1.1 Traslación</a></li>
      <li><a href="#212-escalamiento">2.1.2 Escalamiento</a></li>
      <li><a href="#213-rotación">2.1.3 Rotación</a></li>
      <li><a href="#214-sesgado">2.1.4 Sesgado</a></li>
    </ul>
  </li>
  <li><a href="#22-representación-matricial-de-las-transformaciones-bidimensionales">2.2 Representación matricial de las transformaciones bidimensionales</a></li>
  <li><a href="#23-trazo-de-líneas-curvas">2.3 Trazo de líneas curvas</a>
    <ul>
      <li><a href="#231-bézier">2.3.1 Bézier</a></li>
      <li><a href="#232-b-spline">2.3.2 B-spline</a></li>
    </ul>
  </li>
  <li><a href="#24-fractales">2.4 Fractales</a></li>
  <li><a href="#25-uso-y-creación-de-fuentes-de-texto">2.5 Uso y creación de fuentes de texto</a></li>
</ul>

---

## 2.1 Transformación bidimensional

Las transformaciones bidimensionales son operaciones matemáticas fundamentales en la graficación por computadora que permiten modificar objetos definidos en el plano cartesiano. Estas operaciones actúan sobre los puntos que conforman un objeto, generando una nueva configuración espacial sin necesidad de redefinir completamente su estructura geométrica.

Desde un enfoque formal, una transformación puede definirse como una función:

T: ℝ² → ℝ²

la cual asigna a cada punto (x, y) un nuevo punto (x', y'). Esta definición permite tratar a las transformaciones como aplicaciones matemáticas que pueden analizarse, combinarse y optimizarse.

En el ámbito de la computación gráfica, las transformaciones se utilizan en múltiples etapas del pipeline gráfico, incluyendo el modelado, la visualización y la animación. Permiten realizar operaciones como mover objetos, cambiar su tamaño, rotarlos o deformarlos.

### Clasificación de transformaciones

Las transformaciones bidimensionales pueden clasificarse en:

- **Transformaciones rígidas:** Conservan distancias y ángulos.
  - Traslación
  - Rotación

- **Transformaciones afines:** Conservan paralelismo, pero no necesariamente distancias ni ángulos.
  - Escalamiento
  - Sesgado

- **Transformaciones compuestas:** Resultado de la combinación de varias transformaciones.

Esta clasificación es importante porque determina qué propiedades geométricas se preservan durante la transformación.

---

## 2.1.1 Traslación

La traslación es una transformación que desplaza todos los puntos de un objeto en una dirección determinada sin alterar su forma, tamaño ni orientación. Matemáticamente, se interpreta como la suma de un vector constante a cada punto del objeto.

Esta transformación tiene las siguientes propiedades:

- Conserva distancias entre puntos.
- Conserva ángulos.
- Mantiene la orientación del objeto.
- Es independiente de la forma geométrica.

En términos geométricos, la traslación no introduce distorsión alguna, lo que la convierte en una herramienta esencial para el posicionamiento de objetos dentro de una escena.

### Aplicaciones

- Movimiento de objetos en animaciones.
- Reubicación de elementos en interfaces gráficas.
- Simulación de trayectorias.

---

## 2.1.2 Escalamiento

El escalamiento modifica el tamaño de un objeto mediante factores de escala aplicados a sus coordenadas. Dependiendo de los valores utilizados, puede provocar ampliación, reducción o inversión.

### Tipos de escalamiento

- **Uniforme:** Mantiene las proporciones del objeto.
- **No uniforme:** Distorsiona el objeto al aplicar diferentes factores.

### Propiedades

- No conserva distancias.
- No conserva ángulos (en el caso no uniforme).
- Conserva paralelismo.
- Puede cambiar la orientación si se utilizan factores negativos.

### Consideraciones importantes

El escalamiento depende del punto de referencia. Cuando se realiza respecto al origen, el comportamiento es directo; sin embargo, si se requiere escalar respecto a otro punto, se deben aplicar transformaciones adicionales.

### Aplicaciones

- Zoom en gráficos.
- Ajuste de tamaños en interfaces.
- Modelado geométrico.

---

## 2.1.3 Rotación

La rotación es una transformación que gira un objeto alrededor de un punto fijo. Generalmente, este punto es el origen, aunque en aplicaciones prácticas se utilizan centros arbitrarios.

### Propiedades

- Conserva distancias.
- Conserva ángulos.
- Conserva la forma del objeto.
- Cambia la orientación.

La rotación es una transformación lineal cuando se realiza respecto al origen. En caso contrario, se requiere una combinación de traslaciones.

### Importancia

La rotación es esencial en simulaciones físicas, animación y modelado, ya que permite representar cambios de orientación de manera realista.

---

## 2.1.4 Sesgado

El sesgado es una transformación que altera la forma de un objeto desplazando sus puntos en función de su posición respecto a un eje.

### Propiedades

- No conserva ángulos.
- Conserva paralelismo.
- Introduce distorsión controlada.

### Interpretación geométrica

El sesgado puede entenderse como una transformación que inclina el sistema de coordenadas, generando una apariencia de deformación.

### Aplicaciones

- Simulación de perspectiva.
- Diseño tipográfico.
- Efectos visuales en gráficos.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f246be78-f33c-40cf-8ba9-cc95af7575bf" />


---

## 2.2 Representación matricial de las transformaciones bidimensionales

La representación matricial permite expresar las transformaciones como productos de matrices y vectores, lo que facilita su implementación en sistemas computacionales.

### Coordenadas homogéneas

Para unificar todas las transformaciones, se utilizan coordenadas homogéneas, agregando una tercera componente. Esto permite representar la traslación dentro del mismo marco matemático.

### Ventajas

- Permite combinar transformaciones en una sola matriz.
- Reduce el costo computacional.
- Facilita la implementación en hardware gráfico.
- Proporciona una base algebraica sólida.

### Transformaciones compuestas

Una característica clave es que múltiples transformaciones pueden combinarse mediante multiplicación matricial. El orden de aplicación es crucial, ya que la multiplicación de matrices no es conmutativa.

<img width="1265" height="701" alt="image" src="https://github.com/user-attachments/assets/fb9a5c5d-be63-4aa2-8abf-4a0e0f0ab595" />


---

## 2.3 Trazo de líneas curvas

El trazo de curvas es fundamental para representar objetos complejos con suavidad y precisión. A diferencia de las líneas rectas, las curvas permiten describir formas naturales y orgánicas.

### Características generales

- Continuidad geométrica.
- Suavidad en la transición entre segmentos.
- Control mediante parámetros.

Las curvas son ampliamente utilizadas en diseño gráfico, animación y modelado.

---

## 2.3.1 Bézier

Las curvas de Bézier se definen mediante puntos de control que determinan su forma. No necesariamente pasan por todos los puntos, pero se ven influenciadas por ellos.

### Propiedades

- Están contenidas dentro del polígono de control.
- Presentan continuidad suave.
- Son fáciles de manipular.

### Ventajas

- Control intuitivo.
- Implementación sencilla.
- Amplio uso en aplicaciones gráficas.

### Desventajas

- Falta de control local en curvas de alto grado.

---

## 2.3.2 B-spline

Las curvas B-spline son una generalización que permite mayor flexibilidad.

### Propiedades

- Control local.
- Alta suavidad.
- Independencia parcial de los puntos de control.

### Ventajas sobre Bézier

- Mejor manejo de curvas complejas.
- Mayor estabilidad numérica.
- Flexibilidad en el diseño.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/b245feb8-0a82-421d-a495-cdb0b4ac294c" />


---
# 🐦 Animación 2D en Blender (Grease Pencil) – Ciclo de Vuelo de un Pájaro

##  Descripción

Este proyecto consiste en la creación de una animación 2D por fotogramas utilizando la herramienta **Grease Pencil de Blender**, representando un ciclo básico de vuelo de un pájaro.
El objetivo es demostrar principios fundamentales de animación como el *timing*, la repetición cíclica y la fluidez del movimiento.

---

##  Objetivo

Desarrollar una animación sencilla (máximo 5–6 fotogramas clave) que simule el aleteo de un pájaro en un ciclo continuo.

---

## Herramientas utilizadas

* Blender
* Grease Pencil
* Línea de tiempo (Timeline)
* Onion Skin (Piel de cebolla)

---

##  Procedimiento

### 1. Configuración inicial

* Abrir Blender y eliminar los objetos por defecto.
* Agregar un objeto **Grease Pencil**:

<img width="1516" height="740" alt="image" src="https://github.com/user-attachments/assets/c8d7afe1-1ef3-43c8-9028-ccb2d4f7ada5" />


  * `Shift + A → Grease Pencil → Blank`
* Cambiar al modo **Dibujo/Draw**.

<img width="1508" height="733" alt="image" src="https://github.com/user-attachments/assets/f539ae3a-2872-479a-add6-254d4dc35fff" />

* Agregar una imagen de referencia de los frames, y bajar la opacidad.

<img width="1510" height="765" alt="image" src="https://github.com/user-attachments/assets/eabc5866-e88c-457b-96aa-fb68ad73da41" />


---

### 2. Organización de capas

* Crear al menos dos capas:

  * **Líneas** (contornos)
  * **Relleno** (colores)
* Ajustar el grosor del trazo para lograr un estilo limpio.

<img width="1918" height="742" alt="image" src="https://github.com/user-attachments/assets/519d98b2-08ea-4ef0-b31f-c4f7996e969f" />

---

### 3. Creación del primer fotograma

* Ubicarse en el frame 1.
* Insertar un keyframe:

  * Presionar `I → Drawings`
* Dibujar el pájaro con las alas en posición inicial (por ejemplo, arriba).

<img width="1072" height="967" alt="image" src="https://github.com/user-attachments/assets/2a5475ad-f8ff-4900-82d6-4f5b0a2dd370" />


---

### 4. Activación de Onion Skin

* Activar la opción **Onion Skin** para visualizar fotogramas anteriores.
* Ajustar la opacidad para facilitar la referencia del movimiento.

<img width="1521" height="927" alt="image" src="https://github.com/user-attachments/assets/52528aed-9ca2-4bcd-8c75-f0d4af589fde" />


---

### 5. Creación de los fotogramas siguientes

Para cada nuevo fotograma:

1. Avanzar en la línea de tiempo (por ejemplo, cada 3 frames).
2. Insertar un nuevo keyframe (`I → Drawings`).
3. Dibujar la siguiente posición del movimiento:

* Frame 1: Alas arriba
* Frame 2: Alas bajando
* Frame 3: Alas abajo
* Frame 4: Alas subiendo
* Frame 5: Regreso a la posición inicial

---

### 6. Ajuste del timing

* Espaciar los fotogramas para controlar la velocidad del movimiento.
* Reproducir la animación y ajustar según sea necesario.

<img width="1506" height="901" alt="image" src="https://github.com/user-attachments/assets/13c751ec-b8dc-43e7-8f04-e8900217d5b6" />


---


## Resultado esperado

Una animación fluida de un pájaro en vuelo, que pueda reproducirse en bucle continuo, demostrando principios básicos de animación 2D.

---

## Recomendaciones

* Mantener proporciones consistentes entre fotogramas.
* Usar formas simples para facilitar el proceso.
* Aplicar ligeras variaciones en el cuerpo para dar sensación de movimiento natural.

---

## Notas adicionales

Este proyecto está enfocado en el aprendizaje de animación por fotogramas, por lo que prioriza la claridad del movimiento sobre el nivel de detalle visual. El nivel de detalle puede variar de acuerdo con los resultados deseados.

---


## 2.4 Fractales

Los fractales son estructuras geométricas complejas generadas mediante procesos iterativos. Su característica principal es la auto-similitud.

### Propiedades

- Auto-similitud.
- Dimensión fractal.
- Complejidad infinita.

### Importancia

Los fractales permiten modelar fenómenos naturales de manera realista sin necesidad de grandes cantidades de datos.

### Aplicaciones

- Generación de paisajes.
- Texturización.
- Simulación de fenómenos naturales.

<img width="640" height="480" alt="image" src="https://github.com/user-attachments/assets/03325ba1-6280-4c80-a813-b61d762aa175" />


---

## 2.5 Uso y creación de fuentes de texto

Las fuentes tipográficas son esenciales en la representación visual de información.

### Tipos de fuentes

- **Mapa de bits**
- **Vectoriales**

### Características importantes

- Legibilidad.
- Espaciado (kerning).
- Proporción.
- Estilo.

### Proceso de creación

- Diseño de glifos.
- Definición de curvas.
- Ajuste de métricas.
- Exportación a formatos estándar.

### Aplicaciones

- Interfaces gráficas.
- Diseño editorial.
- Sistemas digitales.

---

## Referencias (Formato APA)

- Hearn, D., & Baker, M. (2013). *Gráficos por computadora con OpenGL*. Pearson Educación.
- Foley, J. D., van Dam, A., Feiner, S. K., & Hughes, J. F. (1996). *Gráficos por computadora: principios y práctica*. Addison-Wesley.
- Angel, E., & Shreiner, D. (2015). *Gráficos por computadora con OpenGL moderno*. McGraw-Hill.
