#### Actividad 3

``` js
```python
from microbit import *
import utime
import music

uart.init(115200)

STATE_CONFIG = 0
STATE_COUNTDOWN = 1
STATE_EXPLODED = 2

current_state = STATE_CONFIG
initial_time = 20
start_time = 0

disarm_sequence = []
disarm_target = ['A', 'B', 'A', 'S']

event_occurred = False
event_value = None

def check_prefix(seq, target):
    return seq == target[:len(seq)]

def tareaEventos():
    global event_occurred, event_value
    if event_occurred:
        return
    if button_a.was_pressed():
        event_value = 'A'
        event_occurred = True
        utime.sleep(0.2)
        return
    if button_b.was_pressed():
        event_value = 'B'
        event_occurred = True
        utime.sleep(0.2)
        return
    if accelerometer.was_gesture("shake"):
        event_value = 'S'
        event_occurred = True
        utime.sleep(0.2)
        return
    if pin_logo.is_touched():
        while pin_logo.is_touched():
            utime.sleep(0.05)
        event_value = 'T'
        event_occurred = True
        utime.sleep(0.2)
        return
    if uart.any():
        c = uart.read(1)
        if c:
            ch = c.decode('utf-8') if isinstance(c, bytes) else str(c)
            if ch in ['A', 'B', 'S', 'T']:
                event_value = ch
                event_occurred = True
                utime.sleep(0.2)
                return

def tareaBomba():
    global current_state, initial_time, start_time, disarm_sequence, event_occurred, event_value
    if current_state == STATE_CONFIG:
        display.show(str(initial_time))
        if event_occurred:
            if event_value == 'A' and initial_time < 60:
                initial_time += 1
                display.show(str(initial_time))
            elif event_value == 'B' and initial_time > 10:
                initial_time -= 1
                display.show(str(initial_time))
            elif event_value == 'S':
                current_state = STATE_COUNTDOWN
                start_time = utime.ticks_ms()
                disarm_sequence = []
            event_occurred = False
            event_value = None
    elif current_state == STATE_COUNTDOWN:
        elapsed = utime.ticks_diff(utime.ticks_ms(), start_time) // 1000
        remaining = initial_time - elapsed
        if remaining < 0:
            remaining = 0
        display.show(str(remaining))
        if event_occurred:
            if event_value in ['A', 'B', 'S']:
                disarm_sequence.append(event_value)
                if not check_prefix(disarm_sequence, disarm_target):
                    disarm_sequence = []
            event_occurred = False
            event_value = None
        if disarm_sequence == disarm_target:
            current_state = STATE_CONFIG
            initial_time = 20
            disarm_sequence = []
            utime.sleep(0.5)
            return
        if remaining == 0:
            current_state = STATE_EXPLODED
            music.play(music.FUNK)
    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
        if event_occurred:
            if event_value == 'T':
                current_state = STATE_CONFIG
                initial_time = 20
                utime.sleep(0.5)
            event_occurred = False
            event_value = None

while True:
    tareaBomba()
    tareaEventos()
    utime.sleep(0.05)
```

En este código, separé la lógica en dos partes: una función (tareaEventos) se encarga de leer todos los eventos que provienen tanto de los sensores del micro:bit como del puerto serial, y almacena el evento en una variable; la otra función (tareaBomba) usa esos eventos para controlar la bomba mediante una máquina de estados. De esta forma, la lógica de la bomba no depende de la fuente del evento, ya que solo lee el valor de la variable, lo que hace el código más flexible y fácil de mantener.


