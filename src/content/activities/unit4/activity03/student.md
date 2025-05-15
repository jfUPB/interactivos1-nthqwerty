#### Actividad 3

```python
from microbit import *

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
    uart.write(data)
    sleep(100)  # Envia datos a 10 Hz
```

**¿Qué datos se están transmitiendo?**
Se envían cuatro valores: las lecturas actuales del acelerómetro en los ejes X (`xValue`) e Y (`yValue`), y los estados de los botones A (`aState`) y B (`bState`). Estos se empaquetan en una cadena de texto separados por comas y terminados con un salto de línea.

**¿Cómo se transmiten los datos?**
La comunicación utiliza el puerto UART del micro\:bit. La línea

```python
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
```

arma la cadena con formato CSV, y `uart.write(data)` la envía. El salto de línea (`\n`) indica el fin de cada paquete.

**Estructura del mensaje**

```
xValue,yValue,aState,bState
```

– Los dos primeros son valores numéricos del acelerómetro.
– Los dos últimos son booleanos (`True`/`False`) según si cada botón está presionado.

**Importancia de las comas y el salto de línea**

* Sin comas, los valores quedarían concatenados y serían difíciles de separar (e.g., `30050TrueFalse`).
* Sin salto de línea, no habría un marcador claro de fin de paquete, complicando el parseo continuo.

**Rol de `sleep(100)`**
Limita el envío a una frecuencia de 10 mensajes por segundo. Sin esa pausa, los datos saldrían ininterrumpidamente, corriendo el riesgo de saturar el UART o perder información.

**Interpretación de `xValue` y `yValue`**

* **Inclinación a la izquierda:** `xValue` negativo, `yValue` cerca de 0
* **Inclinación a la derecha:** `xValue` positivo, `yValue` cerca de 0
* **Pantalla hacia abajo:** `xValue` cerca de 0, `yValue` positivo
* **Pantalla hacia arriba:** `xValue` cerca de 0, `yValue` negativo
* **Plano:** ambos valores cercanos a cero

**Estados de los botones**

* `aState == True` si A está presionado
* `bState == True` si B está presionado
* `False` si no lo están

**Diferencia entre `is_pressed()` y `was_pressed()`**

* `is_pressed()` permanece `True` mientras el botón esté sostenido.
* `was_pressed()` devuelve `True` solo en el instante del clic, luego vuelve a `False` hasta la próxima pulsación.

**Ejemplo de bytes enviados**
Para

```python
xValue = 969
yValue = 652
aState = True
bState = False
```

la cadena es

```
969,652,True,False
```

Convertida a hexadecimal ASCII sería:

```
39 36 39 2C 36 35 32 2C 54 72 75 65 2C 46 61 6C 73 65 0A
```

**Conclusión**
El micro\:bit transmite fácilmente sus lecturas de acelerómetro y el estado de sus botones usando UART. El formato CSV con salto de línea simplifica el parseo por parte de otras plataformas (como p5.js), y el retraso de 100 ms evita la sobrecarga del canal serial.
