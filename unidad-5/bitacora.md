
# Evidencias de la unidad 5

## ACTIVIDAD 01


### 1. Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?

**RTA:** El micro:bit y el sketch en p5.js se comunican a través de un puerto serial abierto a 115200 baudios (miden el número de cambios de señal por segundo en una línea de comunicación, es decir, la velocidad de modulación). El micro:bit toma lecturas de su acelerómetro en los ejes X y Y, y además consulta el estado de los botones A y B. Cada 100 ms (10 Hz) envía una línea con estos cuatro valores concatenados en formato de texto (ASCII), separados por comas y terminados en salto de línea. En otras palabras, el micro:bit manda continuamente mensajes como: "-256,30,False,True"
Esto significa: valor de X = -256, valor de Y = 30, botón A no presionado, botón B presionado.


### 2. ¿Cómo es la estructura del protocolo ASCII usado?

**RTA:** xValue , yValue , aState , bState \n
Los números se transmiten como caracteres, es decir que cada caracter equivale a un byte ASCII, por lo que si se tiene un dato como "-256" como en el ejemplo, su valor será de 4 bytes ASCII. Además los valores se separan por comas y el final de línea se marca con “\n” para que el receptor sepa dónde termina cada mensaje.
Es un protocolo humano-legible, fácil de depurar en un monitor serial, pero poco eficiente porque cada número ocupa varios caracteres en vez de solo unos bytes binarios.


### 3. Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

**RTA:** En el sketch, la lectura ocurre dentro de draw():

<img width="794" height="401" alt="Captura de pantalla 2025-09-19 022837" src="https://github.com/user-attachments/assets/9a602bf2-7252-4b1b-a523-d5506639fafb" />

Cosas por recalcar de esta parte del código y que son muy importantes:

- port.readUntil("\n") recoge la línea completa enviada por el micro:bit.

- split(",") separa los cuatro valores.

- Los valores numéricos X e Y se convierten a enteros y se suman con la mitad del ancho/alto de la ventana, esto on el fin de que el rango del acelerómetro se mapee al centro de la pantalla.

- Los estados de los botones se convierten en booleanos para que cuando no se estén presionando, su estado sea false y cuando si lo estén, se encuentren en true.


### 4. ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?
**RTA:** La función updateButtonStates(newAState, newBState) compara el estado actual de los botones con el estado previo:

- Si detecta que A pasó de false a true, significa que el botón fue presionado. En ese momento el programa cambia el tamaño del módulo gráfico y guarda la posición del clic.

- Si detecta que B pasó de true a false, interpreta que el botón fue soltado y genera un nuevo color aleatorio.

Esto muestra cómo a partir de simples valores booleanos transmitidos en el mensaje serial, el sketch crea “eventos” que afectan la lógica visual.

### 5. Captura de figuras hechas con la app:

<img width="746" height="668" alt="Captura de pantalla 2025-09-19 024232" src="https://github.com/user-attachments/assets/8a059e0f-d98d-49aa-a562-b63cb4dd8ba5" />


**EVALUACIÓN DE LA UNIDAD:** La verdad soy honesto y siento que mi trabajo en esta unidad fue bastante ausente, las situaciones de la semana me retrasaron mucho en mi proceso y a duras penas pude terminar la actividad 1, no obstante, siento que de esa actividad logré comprender el funcionamiento de la recepción de datos por parte del micro:bit y cómo se procesan estos en el código de p5.js, así que como nota me pondría un 1,5 en esta unidad.
