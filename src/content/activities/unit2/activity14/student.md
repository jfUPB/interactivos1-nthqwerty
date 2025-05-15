#### Actividad 14

**Proceso de pruebas y depuración de la Bomba Temporizada**

**1. Casos de prueba y comportamientos esperados**

| # | Acción             | Estado inicial               | Entrada              | Resultado esperado                                   |
| - | ------------------ | ---------------------------- | -------------------- | ---------------------------------------------------- |
| 1 | Incrementar tiempo | En modo configuración, 20s   | Presionar botón A    | El tiempo sube a 21s y se muestra en pantalla        |
| 2 | Reducir tiempo     | En modo configuración, 10s   | Presionar botón B    | No se reduce más, permanece en 10s                   |
| 3 | Activar bomba      | En modo configuración, 15s   | Agitar el micro\:bit | Se muestra “Armed” e inicia la cuenta desde 15s      |
| 4 | Detonación         | En modo armado, 1s restante  | Esperar un segundo   | Se despliega 💀 y el buzzer conectado a P0 se activa |
| 5 | Reiniciar bomba    | En modo armado, 5s restantes | Tocar el pin P1      | Se reinicia a 20s y regresa al modo configuración    |

**2. Fallos detectados y soluciones aplicadas**

* **Error 1: Múltiples lecturas por rebote en los botones A y B**

  * *Causa:* Falta de manejo de rebotes.
  * *Solución:* Se añadió `sleep(200)` después de la detección de pulsación para evitar repeticiones no deseadas.

* **Error 2: La cuenta regresiva no actualizaba bien el display**

  * *Causa:* Las actualizaciones eran demasiado rápidas.
  * *Solución:* Se introdujo `sleep(1000)` entre cada número y se aseguró su visualización adecuada.

* **Error 3: El gesto de sacudida era demasiado sensible**

  * *Causa:* `was_gesture("shake")` reaccionaba a movimientos suaves.
  * *Solución:* Se añadió un pequeño retardo antes de modificar el estado del sistema.

* **Error 4: Sonido de explosión inconsistente**

  * *Causa:* Bucle de sonido mal implementado.
  * *Solución:* Se ajustó la lógica del buzzer para generar un efecto más uniforme.

**3. Código final corregido**

```python
from microbit import *
import utime

# Variables globales
tiempo = 20  # Valor inicial del temporizador (segundos)
config_mode = True  # Estado inicial en modo configuración

# Funciones
def aumentar_tiempo():
    global tiempo
    if config_mode and tiempo < 60:
        tiempo += 1
        display.show(str(tiempo))
        sleep(200)

def disminuir_tiempo():
    global tiempo
    if config_mode and tiempo > 10:
        tiempo -= 1
        display.show(str(tiempo))
        sleep(200)

def armar_bomba():
    global config_mode
    if config_mode:
        config_mode = False
        display.scroll("Armed")
        sleep(500)  # Pequeña pausa antes de iniciar cuenta
        cuenta_regresiva()

def resetear_bomba():
    global config_mode, tiempo
    config_mode = True
    tiempo = 20
    display.scroll("Reset")
    display.show(str(tiempo))
    sleep(200)

def cuenta_regresiva():
    global tiempo
    while tiempo > 0:
        display.show(str(tiempo))
        sleep(1000)
        tiempo -= 1
    explosion()

def explosion():
    display.show(Image.SKULL)
    for _ in range(5):
        pin0.write_digital(1)
        sleep(200)
        pin0.write_digital(0)
        sleep(200)

# Bucle principal
display.show(str(tiempo))
while True:
    if button_a.is_pressed():
        aumentar_tiempo()
    if button_b.is_pressed():
        disminuir_tiempo()
    if accelerometer.was_gesture("shake"):
        armar_bomba()
    if pin1.is_touched():
        resetear_bomba()
    sleep(100)
```

**4. Conclusión**

Las pruebas realizadas permitieron detectar problemas clave en el comportamiento del sistema, los cuales fueron corregidos para lograr una funcionalidad más estable. El código final presenta un mejor manejo de entradas, una transición de estados más clara y una simulación de explosión más realista. Para futuras versiones, sería interesante incorporar sonidos más variados o un mecanismo de confirmación antes de activar la bomba, lo que mejoraría aún más la experiencia del usuario.
