#### Actividad 6

A continuación se muestra un conjunto extenso de vectores de prueba que te ayudarán a verificar el funcionamiento correcto de la bomba, integrando los eventos provenientes de los sensores y del puerto serial. Recuerda que estos vectores se definen en términos de "si estoy en el estado X y recibo el evento Y, entonces debo ejecutar estas acciones y transitar al estado Z". Cada vector de prueba debe considerarse tanto para eventos que provengan del micro:bit (botones, shake, touch) como para los mismos valores recibidos por el puerto serial.

---

### Vectores de Prueba para cada Estado

#### Estado CONFIG (Modo de configuración)
1. **Vector CONFIG-1:**  
   - **Condición Inicial:** STATE_CONFIG, `initial_time = 20`.  
   - **Evento:** Recibo 'A' (ya sea por botón A o serial).  
   - **Acción Esperada:**  
     - Si `initial_time < 60`, incrementa `initial_time` en 1 (de 20 a 21).  
     - Muestra el nuevo valor en el display.  
     - Permanece en STATE_CONFIG.

2. **Vector CONFIG-2:**  
   - **Condición Inicial:** STATE_CONFIG, `initial_time = 60`.  
   - **Evento:** Recibo 'A'.  
   - **Acción Esperada:**  
     - No incrementa el tiempo (ya que 60 es el máximo).  
     - Permanece en STATE_CONFIG.

3. **Vector CONFIG-3:**  
   - **Condición Inicial:** STATE_CONFIG, `initial_time = 20`.  
   - **Evento:** Recibo 'B'.  
   - **Acción Esperada:**  
     - Si `initial_time > 10`, decrementa `initial_time` en 1 (de 20 a 19).  
     - Muestra el nuevo valor en el display.  
     - Permanece en STATE_CONFIG.

4. **Vector CONFIG-4:**  
   - **Condición Inicial:** STATE_CONFIG, `initial_time = 10`.  
   - **Evento:** Recibo 'B'.  
   - **Acción Esperada:**  
     - No decrementa (ya que 10 es el mínimo).  
     - Permanece en STATE_CONFIG.

5. **Vector CONFIG-5:**  
   - **Condición Inicial:** STATE_CONFIG, cualquier `initial_time` válido.  
   - **Evento:** Recibo 'S'.  
   - **Acción Esperada:**  
     - Arma la bomba: transita a STATE_COUNTDOWN.  
     - Registra el tiempo de inicio (`start_time = utime.ticks_ms()`).  
     - Reinicia la secuencia de desarme (disarm_sequence = []).

6. **Vector CONFIG-6:**  
   - **Condición Inicial:** STATE_CONFIG.  
   - **Evento:** Recibo 'T'.  
   - **Acción Esperada:**  
     - No hay acción definida para 'T' en configuración, por lo que se ignora.  
     - Permanece en STATE_CONFIG.

---

#### Estado COUNTDOWN (Cuenta regresiva)
En este estado se muestran dos tipos de acciones: la actualización del tiempo (que sucede de forma automática) y la recolección de eventos para la secuencia de desarme.

7. **Vector COUNTDOWN-1:**  
   - **Condición Inicial:** STATE_COUNTDOWN con `disarm_sequence = []`.  
   - **Evento:** Recibo 'A'.  
   - **Acción Esperada:**  
     - Se añade 'A' a `disarm_sequence` (ahora es ['A']).  
     - Permanece en STATE_COUNTDOWN y continúa la cuenta regresiva.

8. **Vector COUNTDOWN-2:**  
   - **Condición Inicial:** STATE_COUNTDOWN con `disarm_sequence = []`.  
   - **Evento:** Recibo 'B'.  
   - **Acción Esperada:**  
     - La secuencia esperada debe comenzar con 'A'.  
     - Al recibir 'B' directamente, la función check_prefix falla y se reinicia `disarm_sequence` a [] (o se mantiene vacía).  
     - Permanece en STATE_COUNTDOWN.

9. **Vector COUNTDOWN-3:**  
   - **Condición Inicial:** STATE_COUNTDOWN con `disarm_sequence = ['A']`.  
   - **Evento:** Recibo 'B'.  
   - **Acción Esperada:**  
     - Se añade 'B' → `disarm_sequence` pasa a ser ['A', 'B'].  
     - Permanece en STATE_COUNTDOWN.

10. **Vector COUNTDOWN-4:**  
    - **Condición Inicial:** STATE_COUNTDOWN con `disarm_sequence = ['A']`.  
    - **Evento:** Recibo 'A' (en lugar de 'B').  
    - **Acción Esperada:**  
      - La secuencia esperada para el segundo elemento es 'B'; al recibir 'A', la función check_prefix falla y se reinicia `disarm_sequence` a [].  
      - Permanece en STATE_COUNTDOWN.

11. **Vector COUNTDOWN-5:**  
    - **Condición Inicial:** STATE_COUNTDOWN con `disarm_sequence = ['A', 'B']`.  
    - **Evento:** Recibo 'B' (cuando se esperaba 'A').  
    - **Acción Esperada:**  
      - La secuencia esperada para el tercer elemento es 'A'; por recibir 'B', se reinicia `disarm_sequence` a [].  
      - Permanece en STATE_COUNTDOWN.

12. **Vector COUNTDOWN-6:**  
    - **Condición Inicial:** STATE_COUNTDOWN con `disarm_sequence = ['A', 'B']`.  
    - **Evento:** Recibo 'A'.  
    - **Acción Esperada:**  
      - Se añade 'A' → `disarm_sequence` pasa a ser ['A', 'B', 'A'].  
      - Permanece en STATE_COUNTDOWN.

13. **Vector COUNTDOWN-7:**  
    - **Condición Inicial:** STATE_COUNTDOWN con `disarm_sequence = ['A', 'B', 'A']`.  
    - **Evento:** Recibo 'S'.  
    - **Acción Esperada:**  
      - Se añade 'S' → `disarm_sequence` se convierte en ['A', 'B', 'A', 'S'], que coincide con la secuencia de desarme.  
      - Se transita a STATE_CONFIG, se reinicia `initial_time` a 20 y se limpia la secuencia de desarme.

14. **Vector COUNTDOWN-8:**  
    - **Condición Inicial:** STATE_COUNTDOWN sin eventos de desarme y en curso la cuenta regresiva.  
    - **Evento:** Ningún evento de desarme (o eventos no válidos) y el tiempo transcurre hasta que `remaining == 0`.  
    - **Acción Esperada:**  
      - Al agotarse el tiempo (por ejemplo, si el tiempo programado es 20 s y pasan 20 s sin que se complete la secuencia de desarme), se transita a STATE_EXPLODED y se reproduce el sonido de explosión.

15. **Vector COUNTDOWN-9:**  
    - **Condición Inicial:** STATE_COUNTDOWN con cualquier secuencia parcial (por ejemplo, ['A', 'B'])  
    - **Evento:** Recibo 'T' (evento touch).  
    - **Acción Esperada:**  
      - 'T' no se utiliza para la secuencia de desarme en COUNTDOWN, por lo que se ignora y la secuencia permanece sin cambios.  
      - Permanece en STATE_COUNTDOWN.

16. **Vector COUNTDOWN-10 (evento por puerto serial):**  
    - **Condición Inicial:** STATE_COUNTDOWN con `disarm_sequence = []`.  
    - **Evento:** Recibo 'A' proveniente del puerto serial.  
    - **Acción Esperada:**  
      - Se trata igual que si fuera un botón: se añade 'A' a la secuencia → `disarm_sequence` pasa a ser ['A'].  
      - Permanece en STATE_COUNTDOWN.

---

#### Estado EXPLODED (Bomba explotada)
17. **Vector EXPLODED-1:**  
    - **Condición Inicial:** STATE_EXPLODED.  
    - **Evento:** Recibo 'T' (por botón touch o serial).  
    - **Acción Esperada:**  
      - Se reinicia la bomba: transita a STATE_CONFIG, `initial_time` se restablece a 20.  
      - Se muestra el nuevo estado de configuración.

18. **Vector EXPLODED-2:**  
    - **Condición Inicial:** STATE_EXPLODED.  
    - **Evento:** Recibo 'A' (u otro evento que no sea 'T').  
    - **Acción Esperada:**  
      - No hay acción definida en STATE_EXPLODED para eventos distintos de 'T', por lo que se ignora el evento y se permanece en STATE_EXPLODED.

---

### Proceso de Pruebas de Regresión

1. **Implementación de Vectores de Prueba:**  
   - Se simulan los eventos tanto desde los sensores del micro:bit como desde el puerto serial (en el sketch p5.js se enviarán los caracteres 'A', 'B', 'S' y 'T').
   - Se verifica que, para cada vector, el sistema realice las acciones esperadas y transite correctamente entre los estados.

2. **Ejecución y Verificación:**  
   - Se ejecuta el programa en el micro:bit y se disparan manualmente (o mediante una interfaz serial simulada) los eventos según cada vector.
   - Se observa el display, se mide el tiempo transcurrido (para la cuenta regresiva) y se escucha el sonido (cuando la bomba explota).

3. **Ajuste y Repetición:**  
   - Si alguno de los vectores de prueba no se cumple (por ejemplo, la secuencia de desarme incorrecta no se reinicia o no se transita correctamente entre estados), se modifica el código para corregir el comportamiento.
   - Se vuelve a ejecutar todos los vectores de prueba para asegurar que el cambio no afecte a otros casos (pruebas de regresión).

Este enfoque basado en vectores de prueba nos permite cubrir tanto casos límites (por ejemplo, ajustar el tiempo en los límites de 10 y 60 segundos) como escenarios de secuencias correctas e incorrectas en la cuenta regresiva, garantizando que el programa funcione correctamente tanto cuando los eventos provienen del micro:bit como desde el puerto serial.
