#### Actividad 1


```js
from microbit import *
import utime

class Semaforo:
    def __init__(self, x, rojo, amarillo, verde):
        self.x = x
        self.estados = [(9, 0, 0), (9, 9, 0), (0, 9, 0)]  # Rojo, Amarillo, Verde
        self.tiempos = [rojo, amarillo, verde]
        self.estado_actual = 0
        self.inicio_tiempo = utime.ticks_ms()

    def update(self):
        if utime.ticks_diff(utime.ticks_ms(), self.inicio_tiempo) > self.tiempos[self.estado_actual] * 1000:
            self.estado_actual = (self.estado_actual + 1) % 3
            self.inicio_tiempo = utime.ticks_ms()
        self.mostrar()

    def mostrar(self):
        display.clear()
        for i in range(3):
            display.set_pixel(self.x, i, self.estados[self.estado_actual][i])

semaforo1 = Semaforo(0, 5, 2, 3)
semaforo2 = Semaforo(2, 3, 1, 2)
semaforo3 = Semaforo(4, 4, 3, 2)

while True:
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()
    utime.sleep(0.1)
```

El uso de una clase en este caso permite una estructura modular y reutilizable, facilitando la gestión de múltiples semáforos con diferentes tiempos sin duplicar código. Al encapsular la lógica de cada semáforo dentro de una clase, se mejora la organización y mantenimiento del programa. Además, la técnica de programación basada en máquinas de estado es ideal para sistemas secuenciales como este, ya que permite modelar claramente las transiciones entre los estados rojo, amarillo y verde. Esto hace que el código sea más legible, escalable y fácil de depurar, asegurando un control preciso del comportamiento de cada semáforo en concurrencia.
