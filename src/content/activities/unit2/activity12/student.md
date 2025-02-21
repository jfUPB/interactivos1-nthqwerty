#### Actividad 12

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

while True:
    if current_state == STATE_CONFIG:
        display.show(str(initial_time))
        if button_a.was_pressed():
            if initial_time < 60:
                initial_time += 1
                display.show(str(initial_time))
        if button_b.was_pressed():
            if initial_time > 10:
                initial_time -= 1
                display.show(str(initial_time))
        if accelerometer.was_gesture("shake"):
            current_state = STATE_COUNTDOWN
            start_time = utime.ticks_ms()
    
    elif current_state == STATE_COUNTDOWN:
        elapsed = utime.ticks_diff(utime.ticks_ms(), start_time)
        remaining = initial_time - elapsed
        if remaining < 0:
            remaining = 0
        display.show(str(remaining))
        if remaining == 0:
            current_state = STATE_EXPLODED
            music.play(music.FUNK)
        if pin_logo.is_touched():
            while pin_logo.is_touched():
                utime.sleep(50)
            current_state = STATE_CONFIG
            utime.sleep(500)
    
    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
        if pin_logo.is_touched():
            while pin_logo.is_touched():
                utime.sleep(50)
            current_state = STATE_CONFIG
            initial_time = 20
            utime.sleep(500)
```
