#### Actividad 2

``` js
from microbit import *
import utime
import music

STATE_CONFIG = 0
STATE_COUNTDOWN = 1
STATE_EXPLODED = 2

initial_time = 20
current_state = STATE_CONFIG
start_time = 0

disarm_sequence = []
disarm_target = ['A', 'B', 'A', 'S']

def check_prefix(seq, target):
    return seq == target[:len(seq)]

while True:
    if current_state == STATE_CONFIG:
        display.show(str(initial_time))
        if button_a.was_pressed():
            if initial_time < 60:
                initial_time += 1
                display.show(str(initial_time))
                utime.sleep(0.2)
        if button_b.was_pressed():
            if initial_time > 10:
                initial_time -= 1
                display.show(str(initial_time))
                utime.sleep(0.2)
        if accelerometer.was_gesture("shake"):
            current_state = STATE_COUNTDOWN
            start_time = utime.ticks_ms()
            disarm_sequence = []
            utime.sleep(0.2)
    
    elif current_state == STATE_COUNTDOWN:
        if button_a.was_pressed():
            disarm_sequence.append('A')
            if not check_prefix(disarm_sequence, disarm_target):
                disarm_sequence = []
            utime.sleep(0.2)
        if button_b.was_pressed():
            disarm_sequence.append('B')
            if not check_prefix(disarm_sequence, disarm_target):
                disarm_sequence = []
            utime.sleep(0.2)
        if accelerometer.was_gesture("shake"):
            disarm_sequence.append('S')
            if not check_prefix(disarm_sequence, disarm_target):
                disarm_sequence = []
            utime.sleep(0.2)
        
        if disarm_sequence == disarm_target:
            current_state = STATE_CONFIG
            initial_time = 20
            disarm_sequence = []
            utime.sleep(0.5)
            continue

        elapsed = utime.ticks_diff(utime.ticks_ms(), start_time) // 1000
        remaining = initial_time - elapsed
        if remaining < 0:
            remaining = 0
        display.show(str(remaining))
        if remaining == 0:
            current_state = STATE_EXPLODED
            music.play(music.FUNK)
    
    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
        if pin_logo.is_touched():
            while pin_logo.is_touched():
                utime.sleep(0.05)
            current_state = STATE_CONFIG
            initial_time = 20
            utime.sleep(0.5)
    
    utime.sleep(0.05)
```

Implementé la secuencia de desactivación usando una lista para registrar los eventos en el orden en que ocurren. Cada vez que se presiona el botón A, botón B o se detecta un shake, se añade un identificador ('A', 'B' o 'S') a la lista. Con la función auxiliar check_prefix, se verifica si la secuencia actual es un prefijo de la secuencia correcta (A,B,A,S). Si en algún momento la secuencia no coincide, se reinicia. Cuando la lista completa coincide con la secuencia requerida, se desarma la bomba y se vuelve al modo de configuración.
