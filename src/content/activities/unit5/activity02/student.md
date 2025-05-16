#### Actividad 2

### Experimento 1: Visualización en Modo Texto

**Configuración**
Se utilizó el SerialTerminal en “Texto” con el código original modificado para enviar datos en formato binario.

**Resultado**
Aparecen símbolos aleatorios, caracteres de control y espacios vacíos.

**Análisis**
El terminal interpreta cada byte como un carácter ASCII. Dado que los datos son binarios, no corresponden a texto legible, por lo que se muestran valores sin sentido.

> **Ejemplo:** Para `xValue = 1024` (hex `0x0400`), se envían los bytes `0x04` y `0x00`, ambos no imprimibles en ASCII.

---

### Experimento 2: Visualización en Modo “Hex”

**Configuración**
Se cambió el SerialTerminal al modo hexadecimal.

**Resultado**
Se muestran secuencias de bytes en notación HEX, por ejemplo:

```
00 1F 01 A0 00 01
```

que corresponderían a `x=31, y=416, a=0, b=1`.

**Relación con `struct.pack('>2h2B', ...)`**

* `2h`: dos valores de tipo short (2 bytes cada uno) para `xValue` e `yValue`.

  * Ejemplo: `x=31` → `00 1F` (big-endian).
* `2B`: dos valores unsigned char (1 byte cada uno) para `aState` y `bState`.

  * Ejemplo: `a=0, b=1` → `00 01`.

**Lectura**
Aunque compacto, el binario no es directamente comprensible sin decodificarlo.

---

### Ventajas y Desventajas: Binario vs. ASCII

| Criterio          | Binario                | ASCII                                 |
| ----------------- | ---------------------- | ------------------------------------- |
| **Tamaño**        | Más pequeño (6 bytes)  | Más grande (10–15 bytes aprox.)       |
| **Velocidad**     | Transmisión más rápida | Más lento por mayor cantidad de datos |
| **Legibilidad**   | Difícil para humanos   | Claro y autoexplicativo               |
| **Procesamiento** | Directo para máquinas  | Requiere parsing (p.ej. `split(",")`) |

---

### Experimento 3: Envío al Agitar (“shake”)

**Configuración**
El código se modificó para enviar datos únicamente cuando se detecta el gesto de sacudida.

**Resultado**
Cada paquete ocupa 6 bytes:

* 2 bytes para `xValue` (e.g. `0x00 0x1F`)
* 2 bytes para `yValue` (e.g. `0x01 0xA0`)
* 1 byte para `aState` (`0x00`)
* 1 byte para `bState` (`0x01`)

**Estructura `>2h2B`**

* `2h`: 2 valores de 2 bytes → 4 bytes
* `2B`: 2 valores de 1 byte → 2 bytes
* **Total:** 6 bytes por mensaje

**Valores negativos**
Se codifican en complemento a dos.

> Ejemplo: `xValue = -10` → bytes `0xFF 0xF6` (big-endian).

---

### Experimento 4: ASCII vs. Binario Directo

**Configuración**
Al agitar, se envían ambos formatos (texto ASCII y binario) simultáneamente.

**Resultado**

* **ASCII**: `"31,416,0,1\n"` → 10 bytes, legible
* **Binario**: `00 1F 01 A0 00 01` → 6 bytes, compacto

**Comparación**

* **ASCII**

  *  Ideal para depuración rápida
  *  pero Consume más ancho de banda
* **Binario**

  *  Perfecto para altas tasas de envío (p.ej. 10 Hz)
  *  pero Requiere especificar y documentar el protocolo
