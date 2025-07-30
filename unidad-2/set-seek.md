# Unidad 2

## 🔎 Fase: Set + Seek

### Notas de clase del 29/07/2025 (Luego de las notas se encuentra solución de las actividades)

- Significa que se hace un update en cada línea.
- Hay que utilizar técnicas de programación que permitan optimizar el uso de cada segundo.
- FPS: cuantas veces se puede realizar una función por segundo, si se deja medio segundo sin hacer nada, se colapsan los frames a menos de que no sea necesario refrescar la pantalla, por ejemplo, cuando se usa la función "sleep(500)".
- "pixel1.update()" sirve para actualizar la información que se le asigna a los pixeles
- **Instanciar:** es crear un objeto a partir de la clase que nosotros definimos.
- Los datos se inician a través del constructor de la clase, que tiene el mismo nombre de la clase.
  -
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

        

- "mivariable" contiene la dirección de memoria donde se ubica el objeto.
- "this" sirve para guardar la dirección
- Si un objeto fue sobreescrito, el objeto anterior que queda a la deriva es analizado por el software que está corriendo el programa y si ve que no tiene utilidad, lo elimina para darle cupo a nuevos datos.
- "(Self)" puede reemplazar a "this." para guardar la dirección del objeto.
- Un **Objeto** son datos en la memoria.
- "ultimate.ticks_ms()" sirve para que el programa se fije en el tiempo.



### Actividad 1

1. Describe detalladamente como funciona el programa.

RTA: 

  1. Inicialmente en el programa se llama una biblioteca predeterminada, en la que se encuentra la clase "pixel". En está se encuentra el pseudoestado "init" en el que se inicializa el proceso que permite asignarle a los objetos que se vayan a crear más adelante una ubicación en y (pixelX), la ubicación en y (pixelY), un estaado inicial (initState) y un intervalo de tiempo para encender y apagar el led (interval). De igual manera, se encuentra el estado "WaitTimeout" que va de la mano con el método update, este estado permite tomar en cuenta el tiempo que transcurre desde el tiempo en que se inicia el programa, sirviendo para medir los intervalos. Cuando se cumple el tiempo de intervalo, se activa el pixel "display.set_pixel(self.pixelX,self.pixelY,self.pixelState)". Luego se pone un condicional que permite seguir con este loop para que cuando se vuelva a cumplir el intervalo de tiempo, el led se vuelva a encender, en este caso, "utime.ticks_ms()" permite medir si el tiempo de intervalo ya transcurrió y se ponen dos condicionales que representan que si el led está encendido, se apague "if self.pixelState == 9:">>>"self.pixelState = 0" y si es al contrario, se encienda el led.

  2. Por fuera de la biblioteca, se encuentran los objetos creados, en este caso los pixeles que representan a los leds del microbit. Para crearlos como objetos, es necesario crearlos y llamar a la clase pixel para ingresar los datos que le asignan ubicación en X y Y, estado inicial e intervalo de tiempo. Ejemplo: "pixel1 = Pixel(0,0,0,1000)" o "pixel2 = Pixel(4,4,0,500)".

  3. Finalmente se pone el condicional "while true:" que significa que mientras esos datos asignados a los objetos se estén cumpliendo correctamente en el programa, se llame al método update para que los datos recopilados en cada objeto se vayan actualizando mientras que el programa siga corriendo.


4. ¿Cuáles son los estados en el programa?

RTA: Un estado es una **condición de espera**, es decir, qué estoy esperando. En este caso, el programa tiene un pseudoestado que sería "init" y que simboliza que se está inicializando un objeto. Por otro lado el estado "WaitTimeOut" sirve para esperar el momento en que se ejecuta el método.

3. ¿Cuáles son los eventos/inputs en el programa?

RTA: El evento es **preguntar si algo ha ocurrido**. En el programa el evento es cuando pasa el intervalo de tiempo asignado a los objetos, ver si ya pasó el tiempo de espera.

4. ¿Cuáles son las acciones en el programa?

RTA: Acción es **lo que se ejecuta cuando ocurre lo que se está esperando**. En este caso, las acciones del programa son encender o apagar los leds y ver la hora.
