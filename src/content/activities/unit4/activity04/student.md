#### Actividad 4

1. **Revisión del HTML inicial**

   * Se incorporó la biblioteca `p5.webserial.js`
   * Esto habilita la comunicación serial con el micro\:bit usando la WebSerial API

2. **Análisis de recursos gráficos**

   * Archivos incluidos: `02.svg`, `03.svg`, `04.svg`, `05.svg`
   * Estos SVG actúan como módulos de dibujo en el canvas
   * Se puede elegir con las teclas 6–9 (y las teclas 1–4 cambian el color)

3. **Diseño de la máquina de estados**

   * **WAIT\_MICROBIT\_CONNECTION**

     * Espera a que el dispositivo se conecte
     * Muestra un botón “Connect to micro\:bit”
   * **RUNNING**

     * Estado activo de dibujo
     * Procesa los datos entrantes del micro\:bit

4. **Comunicación serial**

   * Velocidad configurada a 115200 baudios
   * Formato de los datos: `"x,y,aState,bState\n"`
   * Flujo de conexión:

     1. El usuario pulsa el botón
     2. Se abre el puerto con `port.open()`
     3. El texto del botón cambia a “Disconnect”
     4. Comienza a recibirse información

5. **Protocolo de intercambio de datos**

   * Los valores se separan con comas y terminan en `\n`
   * Ejemplo de paquete: `"1024,512,true,false\n"`
   * En p5.js se usa `port.readUntil("\n")` para obtener cada paquete completo, luego:

     ```js
     data = data.trim();      // elimina el salto de línea
     parts = data.split(","); // separa en 4 valores
     ```

     y se valida que `parts.length === 4`

6. **Experimentos y ajustes realizados**
   a) **Centro de coordenadas**

   * Sin corrección, `(0,0)` queda en la esquina superior izquierda
   * Con `windowWidth/2`, se centra horizontalmente
   * Con `windowHeight/2`, se centra verticalmente
     b) **Estados de los botones**
   * Al presionar A: genera un tamaño aleatorio (50–160 px) y guarda la posición del clic (`clickPosX`, `clickPosY`)
   * Al soltar B: cambia el color a uno aleatorio con `alpha` entre 80 y 100
     c) **Mensaje de error**: “No se están recibiendo 4 datos del micro\:bit”
   * Causas comunes: paquete incompleto, datos corruptos o desfase temporal

7. **Modificaciones respecto al código original**

   * **Eliminados o alterados**: eventos de ratón (`click`, `drag`), función de la barra espaciadora y algunos atajos de teclado
   * **Conservados**: teclas 1–9 para colores y formas, flechas para ajustar tamaño/velocidad, `S` para guardar el canvas y `Delete`/`Bksp` para limpiar

8. **Hallazgos clave**

   * La comunicación serial funciona de manera asíncrona
   * Es indispensable validar los datos recibidos antes de usarlos
   * Guardar estados anteriores mejora la estabilidad
   * La máquina de estados aporta robustez al flujo
   * Ajustar coordenadas correctamente es fundamental para que el dibujo sea preciso

9. **Mejoras propuestas**

   * Implementar un buffer para manejar datos parciales
   * Añadir un indicador visual de conexión activa
   * Permitir más controles desde el micro\:bit
   * Incorporar historial de acciones (undo/redo)
   * Mejorar la gestión de errores y excepciones

10. **Guía rápida de uso**

    * Pulsar “Connect” para enlazar con el micro\:bit
    * Mover el dispositivo para dibujar en el canvas
    * **Botón A**: cambia el tamaño
    * **Botón B**: cambia el color
    * **Teclas 1–4**: selección de colores
    * **Teclas 6–9**: selección de formas SVG
    * **Flechas**: ajustes finos de tamaño y velocidad
    * **S**: guardar la imagen
    * **Delete**: limpiar el canvas
