# Unidad 2

## 🔎 Fase: Set + Seek

### Notas de clase del 29/07/2025 (Luego de las notas se encuentra solución de las actividades)

- Significa que se hace un update en cada línea.
- Hay que utilizar técnicas de programación que permitan optimizar el uso de cada segundo.
- FPS: cuantas veces se puede realizar una función por segundo, si se deja medio segundo sin hacer nada, se colapsan los frames a menos de que no sea necesario refrescar la pantalla, por ejemplo, cuando se usa la función "sleep(500)".
- "pixel1.update()" sirve para actualizar la información que se le asigna a los pixeles
- **Instanciar:** es crear un objeto a partir de la clase que nosotros definimos.
- Los datos se inician a través del constructor de la clase, que tiene el mismo nombre de la clase.

```python
class Miclase{
    int midata1;
    float midata2;
    string Hola;

  
    void Test(int i){
        midata2 = midata1*i;
    }

}
°
°
°
Miclase mivariable =new Miclase();
mivariable = new Miclase();

        mivariable.Test(25);
```
        

- "mivariable" contiene la dirección de memoria donde se ubica el objeto.
- "this" sirve para guardar la dirección
- Si un objeto fue sobreescrito, el objeto anterior que queda a la deriva es analizado por el software que está corriendo el programa y si ve que no tiene utilidad, lo elimina para darle cupo a nuevos datos.
- "(Self)" puede reemplazar a "this." para guardar la dirección del objeto.
- Un **Objeto** son datos en la memoria.
- "ultimate.ticks_ms()" sirve para que el programa se fije en el tiempo.



### Actividad 01

1. Describe detalladamente como funciona el programa.

RTA: 

  a. Inicialmente en el programa se llama una biblioteca predeterminada, en la que se encuentra la clase "pixel". En está se encuentra el pseudoestado "init" en el que se inicializa el proceso que permite asignarle a los objetos que se vayan a crear más adelante una ubicación en y (pixelX), la ubicación en y (pixelY), un estaado inicial (initState) y un intervalo de tiempo para encender y apagar el led (interval). De igual manera, se encuentra el estado "WaitTimeout" que va de la mano con el método update, este estado permite tomar en cuenta el tiempo que transcurre desde el tiempo en que se inicia el programa, sirviendo para medir los intervalos. Cuando se cumple el tiempo de intervalo, se activa el pixel "display.set_pixel(self.pixelX,self.pixelY,self.pixelState)". Luego se pone un condicional que permite seguir con este loop para que cuando se vuelva a cumplir el intervalo de tiempo, el led se vuelva a encender, en este caso, "utime.ticks_ms()" permite medir si el tiempo de intervalo ya transcurrió y se ponen dos condicionales que representan que si el led está encendido, se apague "if self.pixelState == 9:">>>"self.pixelState = 0" y si es al contrario, se encienda el led.

  b. Por fuera de la biblioteca, se encuentran los objetos creados, en este caso los pixeles que representan a los leds del microbit. Para crearlos como objetos, es necesario crearlos y llamar a la clase pixel para ingresar los datos que le asignan ubicación en X y Y, estado inicial e intervalo de tiempo. Ejemplo: "pixel1 = Pixel(0,0,0,1000)" o "pixel2 = Pixel(4,4,0,500)".

  c. Finalmente se pone el condicional "while true:" que significa que mientras esos datos asignados a los objetos se estén cumpliendo correctamente en el programa, se llame al método update para que los datos recopilados en cada objeto se vayan actualizando mientras que el programa siga corriendo.


2. ¿Cuáles son los estados en el programa?

RTA: Un estado es una **condición de espera**, es decir, qué estoy esperando. En este caso, el programa tiene un pseudoestado que sería "init" y que simboliza que se está inicializando un objeto. Por otro lado el estado "WaitTimeOut" sirve para esperar el momento en que se ejecuta el método.

3. ¿Cuáles son los eventos/inputs en el programa?

RTA: El evento es **preguntar si algo ha ocurrido**. En el programa el evento es cuando pasa el intervalo de tiempo asignado a los objetos, ver si ya pasó el tiempo de espera.

4. ¿Cuáles son las acciones en el programa?

RTA: Acción es **lo que se ejecuta cuando ocurre lo que se está esperando**. En este caso, las acciones del programa son encender o apagar los leds y ver la hora.

### Actividad 02

1. Escribe el código que soluciona este problema en tu bitácora.

RTA: 

```python
from microbit import *
import utime

class Pixel:
    def __init__(self,pixelX,pixelY,initState,interval):
        self.state = "Init"
        self.startTime = 0
        self.interval = interval
        self.pixelX = pixelX
        self.pixelY = pixelY
        self.pixelState = initState
        self.next = None
        
    def get_next(self,next_pixel):
        self.next = next_pixel
                
    def activate(self):
        if self.state == "Init":
            self.state = "Activated"
        
    def update(self):
        if self.state == "Activated":
            self.pixelState = 9 
            self.startTime = utime.ticks_ms()
            display.set_pixel(self.pixelX,self.pixelY,self.pixelState)
            self.state = "Waiting"

        elif self.state == "Waiting":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
                self.pixelState = 0
                display.set_pixel(self.pixelX,self.pixelY,self.pixelState)
                self.state = "Init"
                if self.next:
                    self.next.activate()
                
pixel1 = Pixel(1,3,0,1000)
pixel2 = Pixel(2,2,0,2000)
pixel3 = Pixel(3,1,0,3000)

pixel1.get_next(pixel2)   
pixel2.get_next(pixel3)
pixel3.get_next(pixel1)

pixel1.activate()
while True:
    pixel1.update()
    pixel2.update()
    pixel3.update()
```
2. Identifica los estados, eventos y acciones en tu código.

RTA: 

ESTADOS:

- STATE_INIT (Pseudoestado)
  
- Activated

- Waiting

EVENTOS:

- Esperar a que pasen 3 segundos

- Esperar a que pasen 2 segundos

- Esperar a que pase 1 segundo

ACCIONES:

- Mostrar el pixel 1

- Mostrar el pixel 2

- Mostrar el pixel 3

### Actividad 03

1. Explica por qué decimos que este programa permite realizar de manera concurrente varias tareas.

RTA: Este estado es concurrente porque permite hacer varias tareas tareas de manera multiplexada, es decir, que muchas entidades utilizen un mismo recurso, en este caso la cpu. 

Identifica los estados, eventos y acciones en el programa.

RTA: En este caso, tenemos:

ESTADOS:

- STATE_INIT (Pseudoestado)
  
- STATE_HAPPY
  
- STATE_SMILE
  
- STATE_SAD

EVENTOS:

- Esperar a que pasen 1.5 segundos

- Esperar a que pasen 2 segundos

- Esperar a que pase 1 segundo

- Se presiona el botón A

ACCIONES:

- Mostrar carita feliz

- Mostrar sonrisa

- Mostrar carita triste

2. Describe y aplica al menos 3 vectores de prueba para el programa. Para definir un vector de prueba debes llevar al sistema a un estado, generar los eventos y observar el estado siguiente y las acciones que ocurrirán. Por tanto, un vector de prueba tiene unas condiciones iniciales del sistema, unos resultados esperados y los resultados realmente obtenidos. Si el resultado obtenido es igual al esperado entonces el sistema pasó el vector de prueba, de lo contrario el sistema puede tener un error.

RTA: 

- Si estoy en el estado "STATE_HAPPY" y espero 1.5 segundos se muestra una sonrisa y se pasa al estado "STATE_SMILE".
- Si estoy en el estado "STATE_SAD" y se oprime el botón A, se muestra una sonrisa y se pasa al estado "STATE_SMILE".
- Si estoy en el estado "STATE_SMILE" y espero 1 segundo, se muestra una cara triste y se pasa al estado "STATE_SAD".

```python
from microbit import *
import utime

STATE_INIT=0
STATE_HAPPY=1
STATE_SMILE=2
STATE_SAD=3

currentstate = STATE_INIT 
startTime=0
intervalHappy=1500
intervalSmile=1000
intervalSad=2000

def tarea1():
    global currentstate
    global startTime
    if currentstate==STATE_INIT:
        display.show(Image.HAPPY)
        startTime = utime.ticks_ms()
        currentstate=STATE_HAPPY
        
    elif currentstate==STATE_HAPPY:
        if utime.ticks_diff(utime.ticks_ms(),startTime) > intervalHappy:
            display.show(Image.SMILE)
            startTime = utime.ticks_ms()
            currentstate=STATE_SMILE
            
        if button_a.was_pressed():
            display.show(Image.SAD)
            startTime = utime.ticks_ms()
            currentstate=STATE_SAD
            
    elif currentstate==STATE_SMILE:
        if utime.ticks_diff(utime.ticks_ms(),startTime) > intervalSmile:
            display.show(Image.SAD)
            startTime = utime.ticks_ms()
            currentstate=STATE_SAD

        if button_a.was_pressed():
            display.show(Image.HAPPY)
            startTime = utime.ticks_ms()
            currentstate=STATE_HAPPY
            
    elif currentstate==STATE_SAD:
        if utime.ticks_diff(utime.ticks_ms(),startTime) > intervalSad:
            display.show(Image.HAPPY)
            startTime = utime.ticks_ms()
            currentstate=STATE_HAPPY

        if button_a.was_pressed():
            display.show(Image.SMILE)
            startTime = utime.ticks_ms()
            currentstate=STATE_SMILE
    else:
        display.show(Image.RABBIT)
    
while True:
    tarea1()
```
