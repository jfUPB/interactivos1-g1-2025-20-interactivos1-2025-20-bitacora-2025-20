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
// Generative Design + micro:bit control
// Necesita librería p5.serialport.js

'use strict';

let tileCount = 20;
let actRandomSeed = 0;

let circleAlpha = 130;
let circleColor;

let accelX = 0; // valores recibidos del micro:bit
let accelY = 0;
let aState = false;
let bState = false;

// Serial
let serial;
let latestData = "waiting for data"; 

function setup() {
  createCanvas(600, 600);
  noFill();
  circleColor = color(0, 0, 0, circleAlpha);

  // Inicializar puerto serial
  serial = new p5.SerialPort();
  serial.list();
  serial.open('/dev/ttyUSB0'); // ⚠️ CAMBIA esto según el puerto de tu micro:bit
  serial.on('data', gotData);
}

function draw() {
  translate(width / tileCount / 2, height / tileCount / 2);

  background(255);
  randomSeed(actRandomSeed);

  stroke(circleColor);
  strokeWeight(abs(accelY) / 60); // accelY controla grosor

  for (let gridY = 0; gridY < tileCount; gridY++) {
    for (let gridX = 0; gridX < tileCount; gridX++) {
      let posX = width / tileCount * gridX;
      let posY = height / tileCount * gridY;

      // desplazamiento según accelX
      let shiftX = random(-accelX, accelX) / 20;
      let shiftY = random(-accelX, accelX) / 20;

      // tamaño según accelY
      ellipse(posX + shiftX, posY + shiftY, abs(accelY) / 15, abs(accelY) / 15);
    }
  }

  // Botón A → cambia semilla
  if (aState) {
    actRandomSeed = random(100000);
  }

  // Botón B → guardar PNG
  if (bState) {
    saveCanvas('microbit_art_' + Date.now(), 'png');
  }
}

// Función que recibe los datos seriales
function gotData() {
  let currentString = serial.readLine().trim(); 
  if (!currentString) return;

  let values = currentString.split(",");
  if (values.length === 4) {
    accelX = int(values[0]);
    accelY = int(values[1]);
    aState = (values[2] === "True");
    bState = (values[3] === "True");
  }
}

```

## Video

[Video demostratativo](URL)




