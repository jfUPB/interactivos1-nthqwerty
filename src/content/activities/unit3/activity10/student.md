**Pruebas Iniciales del Sistema**

**Estado: Configuración**

*  Al estar en Configuración y presionar UP (botón A en micro\:bit o flecha ↑ en p5.js), el temporizador debe aumentar en 1 segundo y mantenerse en el mismo estado.
*  Al presionar DOWN (botón B en micro\:bit o flecha ↓ en p5.js) en Configuración, el tiempo se reduce en 1 segundo sin salir de ese modo.
*  Si se intenta bajar el tiempo por debajo de 10 segundos, el valor no debe modificarse.
*  Si se quiere incrementar el tiempo por encima de 60 segundos, tampoco debe cambiar.
*  Si se realiza un movimiento Shake (ARMED en micro\:bit o presionar A en p5.js) estando en Configuración, se debe cambiar al estado Armado y comenzar la cuenta regresiva.

**Estado: Armado**

*  Durante el estado Armado, el temporizador debe reducirse automáticamente en intervalos de un segundo.
*  Cuando el tiempo llegue a cero, se debe pasar al estado de Explosión.
*  Si se presiona el botón Touch (pin0 en micro\:bit o tecla T en p5.js), se debe regresar a Configuración y reiniciar el tiempo a 20 segundos.
*  Los botones UP y DOWN no deben tener ningún efecto mientras se está en el modo Armado.

**Estado: Explosión**

*  En este estado, debe mostrarse un ícono que represente la explosión y emitirse un sonido desde el speaker.
*  Al presionar Touch (pin0 en micro\:bit o tecla T en p5.js), se debe volver a Configuración y reiniciar el tiempo a 20 segundos.
*  Acciones como Shake o presionar UP/DOWN no deben generar cambios mientras se está en el estado de Explosión.

---

**Casos de prueba con fallos y ajustes realizados**

* **Caso 5**: No se producía la transición al estado Armado al agitar el micro\:bit. La solución consistió en revisar correctamente el uso de `was_gesture("shake")` y comprobar la comunicación UART con p5.js.
* **Caso 7**: Al llegar el temporizador a cero, no se producía el cambio al estado Explosión. Se solucionó incluyendo la condición `if tiempo == 0` en el ciclo principal.
* **Caso 8**: El botón de Touch en p5.js no reiniciaba el sistema. La corrección fue enviar el comando `RESET` a través de la conexión UART.

Tras cada corrección, se volvieron a ejecutar todos los casos de prueba desde el inicio para asegurarse de que el sistema funcionara correctamente en su totalidad.

