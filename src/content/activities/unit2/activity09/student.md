#### Actividad 9

``` js

from microbit import *
import utime

RED_INTERVAL = 2000
YELLOW_INTERVAL = 1000
GREEN_INTERVAL = 2000

state = "RED"
state_start = utime.ticks_ms()

while True:
    current_time = utime.ticks_ms()
    if state == "RED":
        display.show("R")
        if utime.ticks_diff(current_time, state_start) > RED_INTERVAL:
            state = "YELLOW"
            state_start = current_time
    elif state == "YELLOW":
        display.show("Y")
        if utime.ticks_diff(current_time, state_start) > YELLOW_INTERVAL:
            state = "GREEN"
            state_start = current_time
    elif state == "GREEN":
        display.show("G")
        if utime.ticks_diff(current_time, state_start) > GREEN_INTERVAL:
            state = "RED"
            state_start = current_time
```


- Estados: Los estados usados son RED, YELLOW y GREEN, cada uno representando el color del semaforo, rojo, amarillo y verde respectivamente.
- Eventos: Se evalua el paso del tiempo con "utime.ticks_diff(current_time, state_start)"
- Acciones: Se USA "display.show()" para mostrar el color del semaforo, por ejemplo: display.show("R") para mostrar el color rojo. Tambien, cada vez q se muestra un color, se pasa el siguiente, y asi sucesivamente hasta haber mostrados los 3 colores, y el programa se reinicia.
