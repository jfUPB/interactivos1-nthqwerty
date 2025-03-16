#### Actividad 5

``` js
from microbit import *

uart.init(115200)

while True:
    if uart.any():  
        display.show(uart.read(1))
```

Para este ejercicio tuve que investigar arduamente sobre los datos seriales y su relacion con el micro:bit. En la primera linea de codigo, se inicializa la comunicacion serial y luego lee el caracter para mostrarlo en la pantalla.
