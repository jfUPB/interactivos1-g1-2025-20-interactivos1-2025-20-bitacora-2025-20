# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01

¿Qué es un sistema físico interactivo?

RTA: Un sistema físico interactivo es una combinación de componentes electrónicos, mecánicos y digitales que responden a estímulos del entorno o del usuario mediante sensores, y generan respuestas a través de actuadores. Básicamente, es un sistema que “siente” su entorno, “piensa” a través de una lógica programada y “actúa” en consecuencia.

¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

RTA: Como estudiante de Ingeniería en Diseño de Entretenimiento Digital, este conocimiento es clave. Por ejemplo, se puede aplicar en el desarrollo de experiencias inmersivas como instalaciones artísticas interactivas, simuladores, o incluso controladores físicos para videojuegos (tipo sensores de movimiento, guantes hápticos o dispositivos MIDI personalizados). También podría usarse en el diseño de prototipos que mezclen el arte digital con componentes electrónicos para experiencias más envolventes.


### Actividad 02

¿Qué es el diseño/arte generativo?

RTA: El diseño de arte generativo es un tipo de arte que se genera a partir de medios autónomos a los cuales se les asigna un algoritmo para que pueda generar un producto a partir de este, sin embargo, el producto generado por este sigue la misma lógica, + más no siempre obtiene el mismo resultado a partir de esta.

¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

RTA: A partir del arte generativo podría generar ideas para que así en caso de tener una idea que aún no tenga buen enfoque, poder plasmarla a base de la aleatoriedad y que tenga diferentes opciones para generar un producto final basándome en los productos que haya obtenido.

### Actividad 03

Después de realizar la actividad, en este sistema físico interactivo identifica los inputs, outputs y el proceso.

RTA: Los inputs del sistema fueron el botón A, el botón B, el "shake" y el botón digital de "send love". En los outputs se encuentra "el mini computador" o el arduino. En el proceso se puede identificar que el funcionamiento del dispositivo es a partir de 3 items importantes: El dispositivo externo (cable y arduino), el editor de p5.js y de micro:bit editor. En este caso, primero se inserta un código en el micro y de ahí, se le envía un algoritmo al arduino, el cual debe estar conectado correctamente al pc. Luego, en P5.js se inserta otro código que es el que va a permitir identificar si el arduino responde al código inicial y en el cual, se pueden modificar las reacciones del arduino a los estímulos que se le hayan asignado en el editor micro. Luego se le da reproducir al P5.js y ahí se puede interactuar con los inputs y outputs del arduino y el pc para ver cuál es la respuesta del dispositivo a cada estímulo digital o externo.

### Actividad 04
Enlace

[Aquí está mi código](https://editor.p5js.org/)

Imagen

<img width="1919" height="1015" alt="Captura de pantalla 2025-07-18 131703" src="https://github.com/user-attachments/assets/22f96c63-7a42-4a1e-953d-58f2706cff5c" />

Código

``` js
let r, g, b;

function setup() {
  createCanvas(600, 400);
  background(0);
  r = random(255);
  g = random(255);
  b = random(255);
}

function draw() {
  background(0);
  fill(r, g, b);
  noStroke();
  ellipse(mouseX, mouseY, 50, 50);
}

function mouseMoved() {
  // Cambia los colores aleatoriamente cuando mueves el mouse
  r = random(255);
  g = random(255);
  b = random(255);
}
```
