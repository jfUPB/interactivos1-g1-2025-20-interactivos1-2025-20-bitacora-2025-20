# Evidencias de la unidad 4
___





___
## Código

[Enlace a la aplicación a modificar](http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_1_2_01)

Código a modificar:

``` js
// P_2_1_2_01
//
// Generative Gestaltung – Creative Coding im Web
// ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
// Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
// with contributions by Joey Lee and Niels Poldervaart
// Copyright 2018
//
// http://www.generative-gestaltung.de
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * changing size and position of circles in a grid
 *
 * MOUSE
 * position x          : circle position
 * position y          : circle size
 * left click          : random position
 *
 * KEYS
 * s                   : save png
 */
'use strict';

var tileCount = 20;
var actRandomSeed = 0;

var circleAlpha = 130;
var circleColor;

function setup() {
  createCanvas(600, 600);
  noFill();
  circleColor = color(0, 0, 0, circleAlpha);
}

function draw() {
  translate(width / tileCount / 2, height / tileCount / 2);

  background(255);

  randomSeed(actRandomSeed);

  stroke(circleColor);
  strokeWeight(mouseY / 60);

  for (var gridY = 0; gridY < tileCount; gridY++) {
    for (var gridX = 0; gridX < tileCount; gridX++) {

      var posX = width / tileCount * gridX;
      var posY = height / tileCount * gridY;

      var shiftX = random(-mouseX, mouseX) / 20;
      var shiftY = random(-mouseX, mouseX) / 20;

      ellipse(posX + shiftX, posY + shiftY, mouseY / 15, mouseY / 15);
    }
  }
}

function mousePressed() {
  actRandomSeed = random(100000);
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}

```

[Enlace a la aplicación modificada](URL)

Código modificado:

``` js
/**
 * Adaptado para micro:bit + p5.webserial
 *
 * MICROBIT
 * acelerómetro x      : posición horizontal de círculos
 * acelerómetro y      : tamaño de los círculos
 * botón A             : cambia la semilla (nuevo patrón)
 * botón B             : guarda PNG
 */

'use strict';

let tileCount = 20;
let actRandomSeed = 0;

let circleAlpha = 130;
let circleColor;

// --- Variables para micro:bit ---
let port;
let connectBtn;
let accelX = 0;
let accelY = 0;
let buttonA = 0;
let buttonB = 0;

function setup() {
  createCanvas(600, 600);
  noFill();
  circleColor = color(0, 0, 0, circleAlpha);

  // Botón de conexión
  port = createSerial();
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(80, 300);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  translate(width / tileCount / 2, height / tileCount / 2);
  background(255);

  // Actualizar estado del botón de conexión
  if (!port.opened()) {
    connectBtn.html('Connect to micro:bit');
  } else {
    connectBtn.html('Disconnect');
  }

  // Leer datos del puerto
  if (port.available() > 0) {
    let data = port.readUntil("\n").trim();
    if (data.length > 0) {
      let values = data.split(",");
      if (values.length >= 4) {
        accelX = int(values[0]);
        accelY = int(values[1]);
        buttonA = int(values[2]);
        buttonB = int(values[3]);
      }
    }
  }

  // Normalizar valores del acelerómetro
  let normX = map(accelX, -1024, 1024, 0, width);
  let normY = map(accelY, -1024, 1024, 0, height);

  randomSeed(actRandomSeed);

  stroke(circleColor);
  strokeWeight(normY / 60);

  for (let gridY = 0; gridY < tileCount; gridY++) {
    for (let gridX = 0; gridX < tileCount; gridX++) {
      let posX = width / tileCount * gridX;
      let posY = height / tileCount * gridY;

      let shiftX = random(-normX, normX) / 20;
      let shiftY = random(-normX, normX) / 20;

      ellipse(posX + shiftX, posY + shiftY, normY / 15, normY / 15);
    }
  }

  // --- Botones del micro:bit ---
  if (buttonA == 1) {
    actRandomSeed = random(100000);
  }
  if (buttonB == 1) {
    saveCanvas('microbit_pattern', 'png');
  }
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
  } else {
    port.close();
  }
}
```
Código 2:
```js
/**
 * Adaptado para micro:bit + p5.webserial
 *
 * MICROBIT
 * acelerómetro x      : posición horizontal de círculos
 * acelerómetro y      : tamaño de los círculos
 * botón A             : cambia la semilla (nuevo patrón)
 * botón B             : guarda PNG
 */

'use strict';

let tileCount = 20;
let actRandomSeed = 0;

let circleAlpha = 130;
let circleColor;

// --- Variables para micro:bit ---
let port;
let connectBtn;
let accelX = 0;
let accelY = 0;
let buttonA = 0;
let buttonB = 0;
let lastA  = 0;
let lastB =0;

function setup() {
  createCanvas(600, 600);
  noFill();
  circleColor = color(0, 0, 0, circleAlpha);

  // Botón de conexión
  port = createSerial();
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(0, 0);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  translate(width / tileCount / 2, height / tileCount / 2);
  background(255);

  // Actualizar estado del botón de conexión
  if (!port.opened()) {
    connectBtn.html('Connect to micro:bit');
  } else {
    connectBtn.html('Disconnect');
  }

  // Leer datos del puerto
  if (port.available() > 0) {
    let data = port.readUntil("\n").trim();
    if (data.length > 0) {
      let values = data.split(",");
      if (values.length >= 4) {
        accelX = int(values[0]);
        accelY = int(values[1]);
        buttonA = int(values[2]);
        buttonB = int(values[3]);
      }
    }
  }

  // Normalizar valores del acelerómetro
  let normX = map(accelX, -1024, 1024, 0, width);
  let normY = map(accelY, -1024, 1024, 0, height);

  randomSeed(actRandomSeed);

  stroke(circleColor);
  strokeWeight(normY / 60);

  for (let gridY = 0; gridY < tileCount; gridY++) {
    for (let gridX = 0; gridX < tileCount; gridX++) {
      let posX = width / tileCount * gridX;
      let posY = height / tileCount * gridY;

      let shiftX = random(-normX, normX) / 20;
      let shiftY = random(-normX, normX) / 20;

      ellipse(posX + shiftX, posY + shiftY, normY / 15, normY / 15);
    }
  }

  // --- Botones del micro:bit ---
  if (buttonA == 1 && lastA === 0) {
    actRandomSeed = random(100000);
  }
lastA = buttonA;
  if (buttonB == 1 && lastB === 0) {
    saveCanvas('microbit_pattern', 'png');
  }
lastB = buttonB;
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
  } else {
    port.close();
  }
}
```
## Video

[Video demostratativo](URL)





