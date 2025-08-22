# Unidad 3


## 🛠 Fase: Apply

### ACTIVIDAD 06

**Código Anterior de Micro:bit:**

```Python
# Imports go at the top
from microbit import *
import utime

display.clear()

class Event:
    def __init__(self):
        self.value = 0

    def write(self,value):
        self.value = value

    def read(self):
        return self.value

    def clear(self):
        self.value = 0

class MicroBitSensors():
    def __init__(self):
        pass

    def update(self):
        if button_a.was_pressed():
            event.write("A")
        if button_b.was_pressed():
            event.write("B")
        if accelerometer.was_gesture('shake'):
            event.write("S")
        if pin_logo.is_touched():
            event.write("T")

class RemoteTask:
    def __init__(self):
        uart.init(baudrate=115200)
    def update(self):
        if uart.any():
            data = uart.read(1)
            if data:
                if data[0] == ord('A'):
                    event.write("A")
                if data[0] == ord('B'):
                    event.write("B")
                if data[0] == ord('S'):
                    event.write("S")
                if data[0] == ord('T'):
                    event.write("T")

class BombTask:
    def __init__(self):
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        display.clear()
        display.show(self.count,wait=False)

    def update(self):
        if self.state == 'CONFIG':
            
            if event.read()=="A":
                event.clear()
                self.count = min(self.count+1,60)
                display.show(self.count,wait=False)

            if event.read()=="B":
                event.clear()
                self.count = max(10,self.count-1)
                display.show(self.count, wait=False)

            if event.read()=="S":
                event.clear()
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count,wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if button_a.was_pressed():
                self.key[self.keyindex] = 'A'
                self.keyindex = self.keyindex + 1

            if button_b.was_pressed():
                self.key[self.keyindex] = 'B'
                self.keyindex = self.keyindex + 1

            if self.keyindex == len(self.key):

                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        passIsOK = False
                        break;
                if passIsOK == True:
                    self.count = 20
                    display.show(self.count,wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0

        elif self.state == 'EXPLODED':
            if event.read()=="T":
                event.clear()
                self.count = 20
                display.show(self.count,wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'

bombTask = BombTask()
event = Event()
sensors = MicroBitSensors()
remotetask =RemoteTask()

while True:
    remotetask.update()
    sensors.update()
    bombTask.update()
```

- Traduzca el código fuente de la bomba en p5.js. No olvides el propósito de esta evaluación: utilizar la técnica de máquina de estados que estamos trabajando en el curso.

**"Intento de traducción 01"**
```js
class Eventos {
  constructor() { // Funciona como init
    this.value = 0;
  }

  escribir(value) {  // Funciona como write
    this.value = value;
  }

  leer() {  // Funciona como read
    return this.value;
  }

  limpiar() {  // Funciona como clear
    this.value = 0;
  }
}

class Bomba {
  constructor() {
    this.contraseña = ['A','B','A']; // Contraseña
    this.clave = new Array(this.contraseña.length).fill(""); 
    this.indiceDeClave = 0;
    this.contador = 20; 
    this.tiempoInicio = millis();
    this.estado = "CONFIGURACIÓN"; 
    console.clear();
  }

  update() {
    if (this.estado === "CONFIGURACIÓN") {
      if (evento.leer() === "A") {
        evento.limpiar();
        this.contador = Math.min(this.contador + 1, 60);
        print(this.contador)
      }

      if (evento.leer() === "B") {
        evento.limpiar();
        this.contador = Math.max(this.contador - 1, 10);
        print(this.contador)
      }

      if (evento.leer() === "S") {
        evento.limpiar();
        this.tiempoInicio = millis();
        this.estado = "ARMADA";
      }
    }

    else if (this.estado === "ARMADA") {
      if (millis() - this.tiempoInicio > 1000) {
        this.tiempoInicio = millis();
        this.contador--;
        print(this.contador);

        if (this.contador === 0) {
          print("KABOOOOOMM!!!");
          this.estado = "EXPLOTÓ";
        }

        // Captura de Contraseña
        if (evento.leer() === "A") {
          this.clave[this.indiceDeClave] = "A";
          this.indiceDeClave++;
        }

        if (evento.leer() === "B") {
          this.clave[this.indiceDeClave] = "B";
          this.indiceDeClave++;
        }

        let contraseñaEstaBien = true; // << lo declaramos aquí
        if (this.indiceDeClave === this.clave.length) {
          for (let i = 0; i < this.clave.length; i++) {
            if (this.clave[i] !== this.contraseña[i]) {
              contraseñaEstaBien = false;
              break;
            }
          }

          if (contraseñaEstaBien) {
            this.contador = 20;
            print(this.contador);
            this.indiceDeClave = 0;
            this.estado = "CONFIGURACIÓN";  
          } else {
            this.indiceDeClave = 0;
          }
        }
      }
    }

    else if (this.estado === "EXPLOTÓ") {
      if (evento.leer() === "T") {
        this.contador = 20;
        print(this.contador);
        this.tiempoInicio = millis();
        this.estado = "CONFIGURACIÓN";
      }
    }
  }
}

let evento;
let bomba;

function setup() {
  createCanvas(400, 400);
  background(220);
  evento = new Eventos();
  bomba = new Bomba();
}

function draw() {
  background(220); 
  bomba.update();
}
```
**"Intento de traducción 2 (Código Funcional)**

```js

let state;
let count;         
let password = [];
let inputKeys = [];
let keyIndex;
let startTime;

// Es como si fuese init
function setup() {
  createCanvas(400, 400);
  background(220); 
  count = 20;    
  state = "CONFIG";
  password = ['A', 'B', 'A'];
  keyIndex=0;
  startTime = millis();
}

function draw() {
  background(220);  

  if (state === "CONFIG") {
    textAlign(CENTER);
    text("A = +1seg", width / 2, 320);
    text("B = -1seg", width / 2, 340);
    text("S = Shake (armar la bomba)", width / 2, 360);
  } 
  
  else if (state === "ARMED") {

    if (millis() - startTime > 1000) {
      startTime = millis();
      count--;
      if (count <= 0) {
        count = 0;
        state = "EXPLODED";
      }
    }
  } 
  
  else if (state === "EXPLODED") {
    text("KABOOOOOM", width / 2, 50);
    text("Apreta T (touch) para reiniciar", width / 2, 90);
  }

  text(count, width / 2, height / 2);
}

//Cuando se presiona una tecla se convierte en mayúscula
function keyPressed() {
  let k = key.toUpperCase();

  if (state === "CONFIG") {
    if (k === 'A') {
      count = min(count + 1, 60);
    } 
    else if (k === 'B') {
      count = max(count - 1, 10);
    } 
    else if (k === 'S') {
      startTime = millis();
      state = "ARMED";
      inputKeys = [];
      keyIndex = 0;
    }
    
  } else if (state === "ARMED") {
    if (k === 'A' || k === 'B') 
    {
      inputKeys[keyIndex] = k;
      keyIndex++;
      checkPassword();
    }
  } else if (state === "EXPLODED") {
    if (k === 'T') {
      count = 20;
      state = "CONFIG";
    }
  }
}

function checkPassword() {
  
  if (keyIndex === password.length) {
    let correct = true;
    for (let i = 0; i < password.length; i++) {
      if (inputKeys[i] !== password[i]) {
        correct = false;
        break;
      }
    }

    if (correct) {
      count = 20;
      inputKeys = [];
      keyIndex = 0;
      state = "CONFIG";
    } else {
      inputKeys = [];
      keyIndex = 0;
    }
  }
}
```
### ACTIVIDAD 07 

**Bomba en p5.js + controles del micro:bit**

Vas a practicar de nuevo la técnica de máquina de estados y eventos genéricos, pero esta vez vas a controlar la bomba desde el micro:bit y desde p5.js. TEN PRESENTE que la bomba estará corriendo en p5.js, pero deberás controlarla también desde los botones del micro:bit mediante el puerto serial.

1. Código p5.js

```js
let eventSystem;
let bomb;

function setup() {
  createCanvas(400, 400);
  textSize(32);
  textAlign(CENTER, CENTER);

  eventSystem = new GameEvent();
  bomb = new BombTask(eventSystem);
}

function draw() {
  background(220);

  bomb.update();

  // Estado en pantalla
  text("Contador: " + bomb.count, width/2, 100);

  if (bomb.state === "EXPLODED") {
    text("BOOM!", width/2, height/2);
  }
}

class GameEvent {
  constructor() {
    this.value = "";
  }
  write(v) {
    this.value = v;
  }
  read() {
    return this.value;
  }
  clear() {
    this.value = "";
  }
}

class BombTask {
  constructor(eventSystem) {
    this.eventSystem = eventSystem;
    this.PASSWORD = ["A", "B", "A"];
    this.key = new Array(this.PASSWORD.length).fill("");
    this.keyindex = 0;
    this.count = 20;
    this.startTime = millis();
    this.state = "CONFIG";
  }

  update() {
    if (this.state === "CONFIG") {
      if (this.eventSystem.read() === "A") {
        this.eventSystem.clear();
        this.count = min(this.count + 1, 60);
      }

      if (this.eventSystem.read() === "B") {
        this.eventSystem.clear();
        this.count = max(10, this.count - 1);
      }

      if (this.eventSystem.read() === "S") {
        this.eventSystem.clear();
        this.startTime = millis();
        this.state = "ARMED";
      }
    }

    else if (this.state === "ARMED") {
      if (millis() - this.startTime > 1000) {
        this.startTime = millis();
        this.count -= 1;

        if (this.count <= 0) {
          this.state = "EXPLODED";
        }
      }

      if (this.eventSystem.read() === "A") {
        this.eventSystem.clear();
        this.key[this.keyindex] = "A";
        this.keyindex++;
      }

      if (this.eventSystem.read() === "B") {
        this.eventSystem.clear();
        this.key[this.keyindex] = "B";
        this.keyindex++;
      }

      if (this.keyindex === this.key.length) {
        let passOK = true;
        for (let i = 0; i < this.key.length; i++) {
          if (this.key[i] !== this.PASSWORD[i]) {
            passOK = false;
            break;
          }
        }

        if (passOK) {
          this.count = 20;
          this.keyindex = 0;
          this.state = "CONFIG";
        } else {
          this.keyindex = 0;
        }
      }
    }

    else if (this.state === "EXPLODED") {
      if (this.eventSystem.read() === "T") {
        this.eventSystem.clear();
        this.count = 20;
        this.startTime = millis();
        this.state = "CONFIG";
      }
    }
  }
}

function keyPressed() {
  if (key === "A") {
    eventSystem.write("A");
  }
  if (key === "B") {
    eventSystem.write("B");
  }
  if (key === "S") {
    eventSystem.write("S");
  }
  if (key === "T") {
    eventSystem.write("T");
  }
}
```

2. Enlace al editor de p5.js con tu código.

<a href="https://editor.p5js.org/ibanezherrerajuandavid17/sketches/e8GicOw-D" target="_códigojs"> </Link del código en p5.js>


3. Código del micro:bit.


```python
# Imports go at the top
from microbit import *
import utime

display.clear()

class Event:
    def __init__(self):
        self.value = 0

    def write(self,value):
        self.value = value

    def read(self):
        return self.value

    def clear(self):
        self.value = 0

class MicroBitSensors():
    def __init__(self):
        pass

    def update(self):
        if button_a.was_pressed():
            event.write("A")
        if button_b.was_pressed():
            event.write("B")
        if accelerometer.was_gesture('shake'):
            event.write("S")
        if pin_logo.is_touched():
            event.write("T")

class BombTask:
    def __init__(self):
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        display.clear()
        display.show(self.count,wait=False)

    def update(self):
        if self.state == 'CONFIG':
            
            if event.read()=="A":
                event.clear()
                self.count = min(self.count+1,60)
                display.show(self.count,wait=False)

            if event.read()=="B":
                event.clear()
                self.count = max(10,self.count-1)
                display.show(self.count, wait=False)

            if event.read()=="S":
                event.clear()
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count,wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if button_a.was_pressed():
                self.key[self.keyindex] = 'A'
                self.keyindex = self.keyindex + 1

            if button_b.was_pressed():
                self.key[self.keyindex] = 'B'
                self.keyindex = self.keyindex + 1

            if self.keyindex == len(self.key):

                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        passIsOK = False
                        break;
                if passIsOK == True:
                    self.count = 20
                    display.show(self.count,wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0

        elif self.state == 'EXPLODED':
            if event.read()=="T":
                event.clear()
                self.count = 20
                display.show(self.count,wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'

bombTask = BombTask()
event = Event()
sensors = MicroBitSensors()

while True:
    sensors.update()
    bombTask.update()
```

