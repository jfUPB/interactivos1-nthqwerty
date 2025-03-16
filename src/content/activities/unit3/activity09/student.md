#### Actividad 9

A continuación se presenta una lista extensa de vectores de prueba y el proceso de regresión para la aplicación de la bomba, que puede recibir eventos desde p5.js o desde el micro:bit vía puerto serial. Estos vectores están definidos para cada uno de los estados (CONFIG, COUNTDOWN, EXPLODED) y consideran tanto eventos modelados como otros inesperados:

---

### Vectores de Prueba

#### **Estado CONFIG (Configuración de la bomba)**
1. **CONFIG-1: Incremento válido**  
   - **Condición Inicial:** Estado CONFIG, `initial_time = 20`.  
   - **Evento:** Recibo 'A' (desde p5.js o micro:bit).  
   - **Acción Esperada:** Incrementa `initial_time` a 21 y muestra el nuevo tiempo.  
   - **Resultado:** Permanece en CONFIG.

2. **CONFIG-2: Incremento en límite máximo**  
   - **Condición Inicial:** Estado CONFIG, `initial_time = 60`.  
   - **Evento:** Recibo 'A'.  
   - **Acción Esperada:** No se incrementa, ya que el tiempo no puede superar 60.  
   - **Resultado:** Permanece en CONFIG con `initial_time` en 60.

3. **CONFIG-3: Decremento válido**  
   - **Condición Inicial:** Estado CONFIG, `initial_time = 20`.  
   - **Evento:** Recibo 'B'.  
   - **Acción Esperada:** Decrementa `initial_time` a 19 y se actualiza el display.  
   - **Resultado:** Permanece en CONFIG.

4. **CONFIG-4: Decremento en límite mínimo**  
   - **Condición Inicial:** Estado CONFIG, `initial_time = 10`.  
   - **Evento:** Recibo 'B'.  
   - **Acción Esperada:** No se decrementa, ya que el tiempo no puede ser menor de 10.  
   - **Resultado:** Permanece en CONFIG.

5. **CONFIG-5: Armar la bomba**  
   - **Condición Inicial:** Estado CONFIG con cualquier `initial_time` válido.  
   - **Evento:** Recibo 'S'.  
   - **Acción Esperada:** Se transita a STATE_COUNTDOWN, se registra el tiempo actual en `countdownStart` y se reinicia cualquier secuencia de desarme.  
   - **Resultado:** Cambio al estado COUNTDOWN.

6. **CONFIG-6: Evento no modelado (T)**  
   - **Condición Inicial:** Estado CONFIG.  
   - **Evento:** Recibo 'T'.  
   - **Acción Esperada:** No hay acción para 'T' en configuración; el evento se ignora.  
   - **Resultado:** Permanece en CONFIG.

---

#### **Estado COUNTDOWN (Cuenta regresiva)**
7. **COUNTDOWN-1: Transcurrir el tiempo sin eventos**  
   - **Condición Inicial:** Estado COUNTDOWN con `initial_time` configurado (por ejemplo, 20 s) y `countdownStart` registrado.  
   - **Evento:** No se recibe ningún evento durante el tiempo.  
   - **Acción Esperada:** La cuenta regresiva se actualiza correctamente cada segundo.  
   - **Resultado:** Al llegar a 0, se transita a STATE_EXPLODED.

8. **COUNTDOWN-2: Desactivación de la bomba**  
   - **Condición Inicial:** Estado COUNTDOWN en curso de cuenta regresiva.  
   - **Evento:** Recibo 'T' (desde cualquier fuente).  
   - **Acción Esperada:** La bomba se desactiva, se reinicia `initial_time` a 20 y se transita a STATE_CONFIG.  
   - **Resultado:** Cambio a CONFIG.

9. **COUNTDOWN-3: Evento irrelevante (A, B o S)**  
   - **Condición Inicial:** Estado COUNTDOWN.  
   - **Evento:** Recibo 'A' (o 'B' o 'S') que no corresponde a la desactivación.  
   - **Acción Esperada:** El evento es ignorado, sin alterar la cuenta regresiva ni el estado.  
   - **Resultado:** Permanece en COUNTDOWN.

10. **COUNTDOWN-4: Evento desconocido**  
    - **Condición Inicial:** Estado COUNTDOWN.  
    - **Evento:** Recibo un carácter no modelado (por ejemplo, 'X').  
    - **Acción Esperada:** El sistema ignora el evento.  
    - **Resultado:** Permanece en COUNTDOWN.

---

#### **Estado EXPLODED (Bomba explotada)**
11. **EXPLODED-1: Reinicio de la bomba**  
    - **Condición Inicial:** Estado EXPLODED (tiempo agotado).  
    - **Evento:** Recibo 'T'.  
    - **Acción Esperada:** Se reinicia la bomba, se establece `initial_time = 20` y se transita a CONFIG.  
    - **Resultado:** Cambio a CONFIG.

12. **EXPLODED-2: Evento irrelevante en explosión**  
    - **Condición Inicial:** Estado EXPLODED.  
    - **Evento:** Recibo 'A', 'B' o 'S'.  
    - **Acción Esperada:** El sistema ignora estos eventos y permanece en EXPLODED hasta que se reciba 'T'.  
    - **Resultado:** Permanece en EXPLODED.

---

### Proceso de Pruebas de Regresión

1. **Implementación de los Vectores:**  
   - Se simulan cada uno de los eventos tanto desde los botones de p5.js como enviando mensajes por el puerto serial (para representar los sensores del micro:bit).  
   - Cada vector se ejecuta de forma individual, verificando que el display, el tiempo y el estado del sistema se actualicen según lo esperado.

2. **Verificación y Registro:**  
   - Se observa el comportamiento del sketch en p5.js: el display muestra el tiempo correcto, la cuenta regresiva se reduce en forma adecuada, y las transiciones entre estados ocurren en los momentos esperados.  
   - Se registra si algún vector falla (por ejemplo, si se incrementa el tiempo por encima de 60 o un evento inesperado altera el estado).

3. **Corrección de Errores:**  
   - Si alguna prueba falla, se analiza el código para identificar la causa (por ejemplo, un mal manejo del evento o una transición no controlada) y se modifica el código para solucionar el problema.  
   - Una vez corregido, se vuelven a ejecutar todos los vectores de prueba para asegurar que la modificación no afecte otros comportamientos (prueba de regresión).

4. **Iteración:**  
   - Este proceso se repite hasta que todos los vectores de prueba se comporten de la forma esperada, asegurando que la aplicación responde correctamente a los eventos provenientes tanto de los sensores del micro:bit como desde la interfaz de p5.js.

---

Este extenso conjunto de vectores de prueba y el proceso de regresión ayudan a garantizar que la aplicación de la bomba funciona correctamente en todos los escenarios posibles, detectando errores potenciales y permitiendo su corrección antes de poner en producción el sistema.
