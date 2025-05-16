#### Actividad 5

| **Aspecto**     | **Protocolo ASCII (Unidad anterior)**                                        | **Protocolo Binario (Unidad actual)**                                 |
| --------------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Eficiencia**  | Bajo (p. ej. `"500,524,1,0\n"` = 11 bytes)                                   | Alto (6 bytes de datos + 2 bytes de framing = 8 bytes)                |
| **Velocidad**   | Lenta (parsing de cadenas con `split`, conversión con `int()`)               | Rápida (lectura directa de bytes con `DataView`)                      |
| **Facilidad**   | Muy legible en consola, ideal para debug                                     | Más complejo de inspeccionar sin herramienta de debugging binario     |
| **Consumo CPU** | Elevado por transformaciones de texto→número y llamadas a `trim()`/`split()` | Reducido: los datos se procesan en bruto sin conversiones intermedias |

---

**Preguntas**

1. **¿Por qué fue necesario introducir framing?**
   Para alinear correctamente los paquetes y evitar lecturas corruptas (vimos ejemplos como `microBitX: 3073` cuando se empezaba a leer en medio de un mensaje).

2. **¿Cómo funciona el framing?**
   Se añade un **header** fixo (`0xAA`) al principio, seguido de los 6 bytes de datos y un **checksum** al final, todo en un bloque de 8 bytes.

3. **¿Qué es un carácter de sincronización?**
   El byte `0xAA` que identifica el comienzo de cada paquete válido y ayuda a re-sincronizar el buffer.

4. **¿Qué es el checksum?**
   Es la suma de comprobación calculada como `sum(dataBytes) % 256`, que en el micro\:bit hacemos con `checksum = sum(data) % 256`. Sirve para detectar paquetes dañados.

5. **Función concat:**
   `serialBuffer = serialBuffer.concat(newData);`
   Acumula cada fragmento entrante en el buffer, necesario porque la transmisión serial puede dividir los datos en trozos.

6. **Bucle `while (serialBuffer.length >= 8)`:**
   Garantiza que haya al menos 8 bytes (1 header + 6 datos + 1 checksum) antes de procesar un paquete completo; de lo contrario, espera más datos.

7. **Significado de `0xAA`:**
   Es `170` decimal, elegido como header por su baja probabilidad de coincidir con valores de sensor en un paquete de datos.

8. **Funciones `shift` y `continue`:**

   ```js
   serialBuffer.shift();  // elimina el primer byte basura
   continue;              // salta a la siguiente iteración
   ```

   Se usan para descartar bytes hasta hallar el header.

9. **Instrucción `break`:**

   ```js
   if (serialBuffer.length < 8) break;
   ```

   Sale del bucle si no hay suficientes bytes para formar un paquete, evitando lecturas parciales.

10. **Diferencia `slice` vs. `splice`:**

    * `slice(0,8)`: copia los primeros 8 bytes sin alterar el buffer.
    * `splice(0,8)`: extrae y elimina esos 8 bytes del buffer.
      Primero se clona el paquete (slice) y luego se limpia el buffer (splice).

11. **Programación funcional con `reduce`:**

    ```js
    let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;
    ```

    Suma todos los bytes de datos para generar el checksum.

12. **Comparación de checksum:**

    ```js
    if (computedChecksum !== receivedChecksum) continue;
    ```

    Descarta automáticamente los mensajes corruptos (p. ej. por interferencias).

13. **Uso de `DataView`:**

    ```js
    let view = new DataView(buffer);
    microBitX = view.getInt16(0);  // 2 bytes → entero (xValue)
    microBitY = view.getInt16(2);  // 2 bytes → entero (yValue)
    microBitAState = view.getUint8(4) === 1;  // 1 byte → booleano
    microBitBState = view.getUint8(5) === 1;  // 1 byte → booleano
    ```

    Convierte directamente los bytes crudos a tipos adecuados sin parsing de texto.
