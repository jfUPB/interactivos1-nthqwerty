#### Actividad 6

1. **¿Qué es un protocolo de comunicación y por qué es clave en la comunicación serial?**
   Un protocolo define las normas para el intercambio de información entre dispositivos. En serial, garantiza que emisor y receptor interpreten los datos de la misma manera, evitando malentendidos.

2. **¿Por qué usamos comas como separadores en nuestro protocolo ASCII?**
   Las comas funcionan como marcadores que distinguen cada dato en la misma línea.

   ```python
   # micro:bit
   serial.writeLine(x + "," + y + "," + aState + "," + bState)
   ```

3. **¿Por qué es necesario terminar cada mensaje con un carácter de fin?**
   El carácter final señala al receptor el término de un paquete de datos, impidiendo lecturas parciales o desordenadas.

   ```javascript
   // p5.js
   let data = port.readUntil("\n"); // "\n" marca el fin del mensaje
   ```

4. **¿Por qué implementamos una máquina de estados en la versión de p5.js?**
   Para organizar el flujo de la aplicación por fases (por ejemplo, inicio, lectura de datos, dibujo), y gestionar cada etapa de forma independiente.

   ```javascript
   if (estado === "inicio") {
     mostrarPantallaInicio();
   } else if (estado === "lectura") {
     procesarDatos();
   }
   ```

5. **¿Cómo se prepara la información en el micro\:bit para enviarla por serial?**
   Se transforma cada valor en texto, se separa con comas y se añade un salto de línea al final.

   ```python
   serial.writeLine(x + "," + y + "," + aState + "," + bState)
   ```

6. **¿Qué implica que los datos del micro\:bit estén en ASCII?**
   Que cada símbolo se codifica como su correspondiente valor ASCII—por ejemplo, la letra `"A"` se envía como el byte `65`.

7. **¿Por qué en p5.js comprobamos si hay bytes disponibles antes de leer?**
   Para asegurarnos de que llegó un mensaje completo y así evitar errores o datos vacíos.

   ```javascript
   if (port.availableBytes() > 0) {
     let data = port.readUntil("\n");
   }
   ```

8. **¿Cómo eliminamos saltos de línea o espacios extra en un string en p5.js?**
   Usando `.trim()`:

   ```javascript
   let limpio = data.trim();
   ```

9. **¿Qué método de p5.js sirve para dividir una cadena por espacios?**
   La función `.split(" ")` crea un arreglo con cada separado:

   ```javascript
   let partes = cadena.split(" ");
   ```

10. **¿Por qué convertir esas cadenas a enteros en p5.js?**
    Porque todo llega como texto, y para procesarlo (por ejemplo, cálculos o posicionar gráficos) necesitamos valores numéricos:

    ```javascript
    let x = int(partes[0]);
    let y = int(partes[1]);
    ```

11. **Si el micro\:bit envía `xValue: 123, yValue: 756, aState: False, bState: True`, ¿qué bytes se transmiten?**
    Se mandaría la cadena

    ```
    123,756,False,True\n
    ```

    y en ASCII cada carácter equivale a un byte:

    ```
    '1' → 49  
    '2' → 50  
    '3' → 51  
    ',' → 44  
    …   
    '\n' → 10  
    ```

12. **¿Qué descubrí del micro\:bit que no sabía antes?**
    Aprendí a usar el puerto serial para enviar datos al ordenador y a formatearlos con `writeLine()` de forma que sean fáciles de procesar desde otra aplicación.

13. **¿Qué aprendí de p5.js que desconocía?**
    Descubrí `serial.readUntil()` para recibir paquetes completos, `.split()` para separar cadenas y `int()` para convertir texto en números y utilizarlos en gráficos.
