# Unidad 1

## 🛠 Fase: Apply

### Actividad 05

**PREGUNTA:** En tu bitácora: explica cómo funciona el sistema físico interactivo que acabamos de crear

RTA: 

**INPUTS:** El botón A
**OUTPUTS:** Cambio de color en la pantalla
**PROCESO**

Primero, en el código se de debe enunciar la tecla A del microbit para que cuando se oprima este, en la pantalla del pc el cuadrado cambie de color. Luego en la parte de presionar el botón "A" se pone "is_pressed" para que al momento de interactuar con el botón en el microbit, la orden se mantenga cuando uno presiona el botón y no la ejecute de manera rápida y no continua. Cabe recalcar que para esto funcione, también hay que ponerle un tiempo de reacción, enunciado como "sleep" para que cuando se ejecute la acción, la instrucción no sea tan corta. 

from microbit import *

uart.init(baudrate=115200)
display.show(Image.ANGRY)

while True:
    if button_a.is_pressed():
        uart.write('A')
    else:
        uart.write('N')

  sleep(100)
  
  Por otro lado, en el código del editor p5j.s se ponen las instrucciones para que cuando se conecte el microbit al programa y se ejecute, el portal de conección esté abierto y se pueda procesar la instrucción de cambio de color correctamente a la hora de presionar el botón A en el microbit.

let port;
let connectBtn;
let connectionInitialized = false;

function setup() {
  createCanvas(400, 400);
  background(220);
  port = createSerial();
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(80, 300);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);
  
  if (port.opened() && !connectionInitialized) {
      port.clear();
      connectionInitialized = true;
    }
  
  
  
  if (port.availableBytes() > 0) {
      let dataRx = port.read(1);
      if (dataRx == "A") {
        fill("red");
      } else if (dataRx == "N") {
        fill("green");
      }
    }
  
  rectMode(CENTER);
  rect(width / 2, height / 2, 50, 50);

  if (!port.opened()) {
            connectBtn.html("Connect to micro:bit");
  } else {
            connectBtn.html("Disconnect");
  }

  
  
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
      connectionInitialized = false;
    } else {
        port.close();
    }
}

En el código se especifica el tamaño del lienzo, el color al que va a cambiar el cuadrado, en este caso si no se presional el botón, será verde y si se presiona, cambia a rojo, para esto se utiliza un condicional. También se encuentra el código para crear el botón de conectar el microbit y el de finalizar la conección (Siendo el mismo solo que cuando se oprime conectar, al estar conectado la opción pasa de aparecer como conectar a desconectar.)
  

##ACTIVIDAD 06

**1. Enlace al editor p5.js:** [https://editor.p5js.org/ibanezherrerajuandavid17/sketches/MJfj6c-iQ](https://editor.p5js.org/ibanezherrerajuandavid17/sketches/MJfj6c-iQ)

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
