#### Actividad 3

### 1. Formato de texto delimitado vs. formato binario

En la unidad anterior enviábamos datos como texto CSV terminado en `\n` porque ofrecía:

* **Legibilidad**: Podíamos leer y depurar fácilmente los valores tal cual aparecen en pantalla.
* **Flexibilidad**: Cada línea podía variar en longitud según la cantidad de dígitos de los números.
* **Sincronización sencilla**: El carácter de nueva línea marcaba claramente el final de cada mensaje.

Con el formato binario, en cambio:

* **Sin delimitadores**: Al usar paquetes de tamaño fijo (6 bytes), ya no hacen falta comas ni saltos de línea.
* **Mayor eficiencia**: Se evitan bytes redundantes, reduciendo el volumen de datos transmitidos.
* **Precisión numérica**: Los valores enteros viajan en su representación nativa, sin conversión a cadena.

---

### 2. Evolución del código en p5.js

**Versión basada en texto:**

```javascript
let str = port.readUntil("\n");
let values = str.trim().split(",");
microBitX = int(values[0]);
microBitY = int(values[1]);
// ... (parsing de strings)
```

**Nueva versión binaria:**

```javascript
let data = port.readBytes(6);
const view = new DataView(buffer);
microBitX = view.getInt16(0); // Lectura binaria
microBitY = view.getInt16(2);
// ... (acceso directo a bytes)
```

**Cambios esenciales:**

* Se elimina la descomposición de cadenas y la llamada a `int()`.
* Se usan desplazamientos fijos en el buffer (0, 2, 4, 5) para leer cada campo.
* Lectura directa de valores enteros con `getInt16`, garantizando velocidad.

---

### 3. Error de sincronización detectado

En algunos momentos veíamos lecturas absurdas como:

```
microBitX: 3073  microBitY: 1 ...
```

**Causa:** p5.js comenzaba a leer desde un byte intermedio (por ejemplo, byte 2 en lugar de 0). Así, los dos bytes de `xValue` (500 = `0x01F4`) se interpretaban mal (`F4 02` → `0xF402` = 62466).

Para detectar estos paquetes corruptos se introdujo un **checksum** que comprueba la integridad de cada bloque.

---

### 4. Solución con framing (header + checksum)

**En el micro\:bit:**

```python
packet = b'\xAA' + data + bytes([checksum])  # Paquete total de 8 bytes
```

**En p5.js:**

```javascript
// Alinea al header 0xAA
while (serialBuffer[0] !== 0xAA) {
  serialBuffer.shift();
}
// Calcula checksum
let dataBytes = serialBuffer.slice(1, 7);
let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;
if (computedChecksum === serialBuffer[7]) {
  // paquete válido
}
```

**Beneficios observados:**

* **Sincronización fiable**: El byte `0xAA` actúa como punto de anclaje para el inicio del mensaje.
* **Detección de errores**: El checksum descarta automáticamente paquetes con datos corrompidos.
* **Robustez del buffer**: Se acumulan bytes hasta encontrar un paquete completo y válido.

---

### 5. Ajustes finales en ambos programas

* **Micro\:bit**:

  * Sigue usando acelerómetro y botones reales (`accelerometer.get_x()`, `button_a.is_pressed()`).
  * Mantiene el framing con `0xAA` y checksum.

* **p5.js**:

  * Centra las coordenadas con `microBitX + windowWidth / 2`, etc.
  * Se quitan logs innecesarios para limpiar la consola.
  * Solo aparecen lecturas coherentes, por ejemplo:

    ```
    microBitX: 523  microBitY: -102  ...
    ```

---

## Conclusión

Implementar un framing con un **header** (`0xAA`) y un **checksum** ha sido decisivo para:

* **Sincronizar** el flujo de bytes, garantizando que p5.js siempre empiece a leer en el byte correcto.
* **Asegurar la integridad** de los datos, descartando automáticamente paquetes erróneos.
* **Optimizar el ancho de banda**, ya que el protocolo binario transmite menos bytes que la versión ASCII.

Este patrón de comunicación—muy habitual en protocolos industriales y sistemas embebidos—demostró su valor en nuestro proyecto. Además, la combinación de:

* Lectura binaria directa (sin parsers de texto),
* Framing robusto y
* Ajustes mínimos en el código p5.js

garantizó un sistema fluido y resistente a interferencias. Para futuros desarrollos, podríamos añadir:

* **Retransmisiones automáticas** de paquetes perdidos,
* **Indicadores visuales** de estado de conexión y errores,
* **Estadísticas** de latencia y tasa de pérdida de paquetes,

con el fin de monitorizar y afinar aún más nuestra infraestructura de comunicación en tiempo real.
