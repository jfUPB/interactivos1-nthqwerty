#### Actividad 4

**Aplicacion original**

https://p5js.org/examples/repetition-kaleidoscope/

**Aplicacion modificada (Binario)**

https://editor.p5js.org/nthqwerty/sketches/ufX6O861R

**Codigo modificado (parte clave)**

``` javascript
// Buffer para almacenar bytes recibidos
let serialBuffer = [];

function readSerialData() {
  // 1. Acumular bytes recibidos
  let newData = port.readBytes(port.availableBytes());
  serialBuffer = serialBuffer.concat(newData);

  // 2. Procesar paquetes completos (8 bytes)
  while (serialBuffer.length >= 8) {
    // 2.1 Buscar header (0xAA)
    if (serialBuffer[0] !== 0xAA) {
      serialBuffer.shift();
      continue;
    }

    // 2.2 Extraer paquete
    let packet = serialBuffer.slice(0, 8);
    let dataBytes = packet.slice(1, 7);
    let checksum = packet[7];

    // 2.3 Validar checksum
    if (dataBytes.reduce((s, v) => s + v, 0) % 256 !== checksum) {
      console.log("Error de checksum");
      serialBuffer.splice(0, 8);
      continue;
    }

    // 2.4 Decodificar datos
    let view = new DataView(new Uint8Array(dataBytes).buffer);
    microBitX = view.getInt16(0) + width/2;
    microBitY = view.getInt16(2) + height/2;
    microBitAState = view.getUint8(4) === 1;
    microBitBState = view.getUint8(5) === 1;

    // 2.5 Actualizar buffer
    serialBuffer.splice(0, 8);
  }
}
```


Aquí tienes los principales cambios organizados bajo los tres apartados:

---

### Protocolo binario

1. **Lectura de datos en lugar de mouse**:

   * Lee `xValue`, `yValue`, `buttonA`, `buttonB` desde el puerto serial (paquetes de texto ASCII).

2. **Mapeo de acelerómetro**:

   * Convierte rangos (–1024…1024) a coordenadas de lienzo con `map()`, en lugar de desplazar directamente valores de ratón.

3. **Botones virtuales**:

   * `buttonA` y `buttonB` ya no vienen de `mouseIsPressed`, sino del estado de botones enviados por el micro\:bit.

---

### Mecanismos de robustez

1. **Guardado de conexión**:

   * Se omite `draw()` entero hasta que `connectionEstablished` sea `true`, evitando errores antes de conectar.

2. **Buffer y parsing de líneas**:

   * Se acumulan fragmentos en `buffer`, se buscan y consumen paquetes completos terminados en `\n`.
   * Se valida longitud (exactamente 4 campos) antes de asignar valores, descartando líneas corruptas.

3. **Lectura asíncrona controlada**:

   * Uso de `await reader.read()` en bucle, liberación de lock (`reader.releaseLock()`) al cerrar, y `try/catch` para capturar errores de conexión.

---

### Optimizaciones

1. **Suavizado de movimiento**:

   * Sustituye saltos bruscos por `lerp(lastX, mappedX, 0.2)` / `lerp(lastY, mappedY, 0.2)` para transiciones más naturales.

2. **Cambio de simetría controlado**:

   * Modifica `symmetry` solo cada 30 cuadros (`frameCount % 30 === 0`) cuando `buttonB` es `true`, evitando cambios rápidos accidentales.

3. **Colores y tamaños dinámicos**:

   * Calcula `dotSize` y valores RGB con funciones `sin()`/`cos()` variables en el tiempo, en lugar de un simple `stroke(255)` del código original.

4. **Eliminación de llamadas redundantes**:

   * Saca `noStroke()`, `fill()` y `ellipse()` fuera de bucles innecesarios, ahorrando ciclos de renderizado.

5. **Texto de estado optimizado**:

   * Muestra sólo 5 líneas de texto con `toFixed(2)` en `xValue`,`yValue`, estados de botones y simetría, en lugar de mensajes de depuración dispersos.
