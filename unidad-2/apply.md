# Unidad 2


## 🛠 Fase: Apply


### Actividad 04

Diseña la máquina de estados para solucionar el siguiente problema:

En un escape room se requiere construir una aplicación para controlar una bomba temporizada. El circuito de control de la bomba está compuesto por cuatro sensores, denominados UP (botón A), DOWN (botón B), touch (botón de touch) y ARMED (el gesto de shake de acelerómetro). Tiene dos actuadores o dispositivos de salida que serán un display (la pantalla de LEDs) y un speaker.

- El controlador funciona así:

a. Inicia en modo de configuración, es decir, sin hacer cuenta regresiva aún, la bomba está desarmada. El valor inicial del conteo regresivo es de 20 segundos.

b. En el modo de configuración, los pulsadores UP y DOWN permiten aumentar o disminuir el tiempo inicial de la bomba.

c. El tiempo se puede programar entre 10 y 60 segundos con cambios de 1 segundo. No olvides usar utime.ticks_ms() para medir el tiempo. Además, 1 segundo equivale a 1000 milisegundos.

d. Hacer shake (ARMED) arma la bomba, es decir, inicia el conteo regresivo.

e. Una vez armada la bomba, comienza la cuenta regresiva que será visualizada en la pantalla de LED.

f. La bomba explotará (speaker) cuando el tiempo llegue a cero.

g. Para volver a modo de configuración deberás tocar el botón touch.

**DIAGRAMA DE ESTADOS/EVENTOS/ACCIONES**

![Diagrama de estados, eventos y acciones](C:\Users\Computador\Pictures\Screenshots)


### ACTIVIDAD 05

Implementa el código para la bomba temporizada usando mycropython y el micro:bit, incluyendo la funcionalidad básica: configuración del tiempo, cuenta regresiva y detonación. Reporta en un tu bitácora lo siguiente:

1. El código que implementa la bomba temporizada.

```Python
from microbit import *
import utime

# Definimos los estados
STATE_INIT = 0
STATE_CONFIGURACION = 1
STATE_CUENTAREGRESIVA = 2
STATE_EXPLOSION = 3

# Estado inicial
state = STATE_INIT
start_time = 0
cuentaRegresiva = 20  # Valor inicial

while True:
    # --- STATE_INIT ---
    if state == STATE_INIT:
        display.scroll("START")
        state = STATE_CONFIGURACION

    
    elif state == STATE_CONFIGURACION:
        display.show(str(cuentaRegresiva))  # Muestra el tiempo configurado

        if button_a.was_pressed():
            if cuentaRegresiva < 60: # Aumentar tiempo con botón A (máx 60)
                cuentaRegresiva += 1
                display.show(str(cuentaRegresiva))
                utime.sleep_ms(200)  # Anti rebote para que registre cuando se presiona el botón A

        if button_b.was_pressed():
            if cuentaRegresiva > 10: # Disminuir tiempo con botón B (mín 10)
                cuentaRegresiva -= 1
                display.show(str(cuentaRegresiva))
                utime.sleep_ms(200)  # Anti rebote para que registre cuando se presiona el botón B

        if accelerometer.was_gesture("shake"): # Iniciar cuenta regresiva con agitar
            start_time = utime.ticks_ms()
            state = STATE_CUENTAREGRESIVA

            
    elif state == STATE_CUENTAREGRESIVA:
        display.show(str(cuentaRegresiva)) #Mostrar tiempo de cuentaRegresiva en leds (se actualiza cada seg que pasa)
        
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= 1000: #Si pasa 1 seg, se reduce de a 1 el valor de cuentaRegresiva y eso se ve en pantalla
            cuentaRegresiva -= 1
            start_time = utime.ticks_ms() #Compara cuantos segundos han pasado desde el inicio de la cuenta regresiva
        if cuentaRegresiva <= 0:
            state = STATE_EXPLOSION

    
    elif state == STATE_EXPLOSION:
        display.show(Image.SKULL)
        pin0.write_digital(1)  # Se activa bocina
        
        if pin_logo.is_touched():
            cuentaRegresiva = 20  # Vuelve el contador a 20 seg
            state = STATE_CONFIGURACION #regresa al estado configuración
```

2. La definición de los vectores de prueba básicos que permiten verificar el correcto funcionamiento del programa.

a. Si se agita el micro:bit, se pasa del estado STATE_CONFIGURACION al estado STATE_CUENTAREGRESIVA y en pantalla se muestra la cuenta regresiva ajustada

b. Se espera a que la cuenta regresiva llegue a 0 y cuando esto ocurra, se pasa de STATE_CUENTAREGRESIVA a STATE_EXPLOSION y se activa el speaker mientras que e pantalla se ve en led una calavera.

c. Si en STATE_EXPLOSION se presiona el "touch", pasa de STATE_EXPLOSION a STATE_CONFIGURACION, el speaker se calla y el tiempo de la cuenta regresiva se reinicia a 20, mostrando en pantalla el número 20 como al inicio.

d. Si en STATE_CONFIGURACION se presiona el botón A, se suma 1 seg al valor de la cuenta regresiva y se muestra en pantalla se muestra el valor de los segundos con los que queda ajustada la cuenta regresiva.

e. d. Si en STATE_CONFIGURACION se presiona el botón B, se resta 1 seg al valor de la cuenta regresiva y se muestra en pantalla se muestra el valor de los segundos con los que queda ajustada la cuenta regresiva.


