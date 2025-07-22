# Unidad 1

## 🛠 Fase: Apply

### Actividad 05

**PREGUNTA:** En tu bitácora: explica cómo funciona el sistema físico interactivo que acabamos de crear

RTA: 

##ACTIVIDAD 06

**1. Enlace al editor p5.js:** https://editor.p5js.org/ibanezherrerajuandavid17/sketches/MJfj6c-iQ

**2. Código del programa:**

let port;
let reader;
let circleX;
let circleColor;
let step = 20;

function setup() {
  createCanvas(800, 400);
  circleX = width / 2;
  circleColor = color(127);

  let connectButton = createButton("Connect to micro:bit");
  connectButton.position(10, 10);
  connectButton.mousePressed(connectToMicrobit);
}

function draw() {
  background(240);
  fill(circleColor);
  noStroke();
  ellipse(circleX, height / 2, 120, 120);
}

async function connectToMicrobit() {
  try {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });

    const decoder = new TextDecoderStream();
    const inputDone = port.readable.pipeTo(decoder.writable);
    const inputStream = decoder.readable;

    reader = inputStream.getReader();

    readSerialLoop();
  } catch (err) {
    console.error("Error al conectar con micro:bit:", err);
  }
}

async function readSerialLoop() {
  while (true) {
    const { value, done } = await reader.read();
    if (done) {
      console.log("Serial desconectado");
      break;
    }
    if (value) {
      handleSerial(value.trim());
    }
  }
}

function handleSerial(data) {
  if (data === "A") {
    circleX -= step;
    circleColor = color(255, 0, 0); // Rojo
  } else if (data === "B") {
    circleX += step;
    circleColor = color(0, 0, 255); // Azul
  }

  circleX = constrain(circleX, 60, width - 60);
}

**3. Código del Microbit**

from microbit import *

uart.init(baudrate=115200)
display.show(Image.ANGRY)

while True:
    if button_a.was_pressed():
        uart.write('A')
    if button_b.was_pressed():
        uart.write('B')

    sleep(100) 
