#### Actividad 3

4 dispositivos de entrada del micro:bit.
- Botones: Los botones A, B y virtual nos permite interactuar fisicamente con el micro:bit para mandar señales.
- Acelerometro: El acelerometro nos permite hacer cosas como agitar el micro:bit para mandar señales.
- Microfono: Sirve para detectar frecuencias de sonido.
- Brujula: Permite detectar campos magneticos.

4 dispositivos de salida del micro:bit.
- Pantalla LED: Son 25 LEDs que actuan acorde al programa.
- Radio: Este funciona como dispositivo de entrada y salida, con el proposito principal de comunicarse con otros micro:bits.
- Altavoz: Reproduce frecuencias de sonido.
- Pines: Puedes conectar dispositivos de entrada y salida al micro:bit, como audifonos.

- Entradas
(Botones A y B)
``` js
from microbit import *

while True:
    if button_a.is_pressed():
        display.show("A")
    elif button_b.is_pressed():
        display.show("B")
    else:
        display.clear()
```

(Agitar)
``` js
from microbit import *

while True:
    if accelerometer.was_gesture('shake'):
        display.show(Image.SURPRISED)
    sleep(100)
```

(Brujula)
``` js
from microbit import *

compass.calibrate()

while True:
    heading = compass.heading()
    display.scroll(str(heading))
    sleep(1000)

```


- Salidas
(Pantalla)
``` js
from microbit import *

display.show(Image.HEART)
sleep(1000)
display.clear()
```

(Altavoz)
``` js
from microbit import *
import music

# Reproduce una melodía sencilla
music.play(music.BA_DING)
```

(Radio)
``` js
from microbit import *
import radio

radio.on()  # Enciende la radio
radio.config(channel=7)  # Configura el canal (opcional, por defecto es 7)

while True:
    radio.send('Hola')  # Envía el mensaje "Hola"
    sleep(1000)
```
