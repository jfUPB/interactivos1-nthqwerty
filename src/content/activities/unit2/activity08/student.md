#### Actividad 8

En el codigo presentado se crea un sistema donde dos píxeles de la pantalla LED del micro:bit parpadean de forma independiente en intervalos diferentes. Primero, el codigo define una clase llamada Pixel, que controla el parpadeo de un pixel en la pantalla del micro:bit. Inicialmente, el píxel se muestra con un brillo definido y se guarda el tiempo de inicio. Luego, cuando transcurre un intervalo específico, se alterna su brillo entre 0 y 9 y se actualiza la pantalla. Dos instancias, con posiciones e intervalos distintos, permiten que dos píxeles parpadeen de forma independiente.

- Estados: "Init" representa el estado inicial donde se enciende el pixel con el brillo definido. "WaitTimeout" espera el tiempo definido antes de cambiar el brillo.
- Eventos: Se usa "utime.ticks_ms()" para medir el tiempo y asi comparar si ha pasado el intervalo especifico.
- Acciones: Encender o apagar el pixel en la posicion especifica con "display.set_pixel()" y cambiar el estado del pixel alternando el brillo entre 9 (encendido) y 0 (apagado).
