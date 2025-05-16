#### Actividad 1

### 1. Información que envía el micro\:bit

El micro\:bit transmite de forma constante cuatro valores a través del puerto UART:

* **xValue**: lectura del acelerómetro en el eje X
* **yValue**: lectura del acelerómetro en el eje Y
* **aState**: estado del botón A (`True` si está pulsado, `False` en caso contrario)
* **bState**: estado del botón B (`True` si está pulsado, `False` en caso contrario)

Estos datos viajan codificados en ASCII como una única línea de texto:

```
"xValue,yValue,aState,bState\n"  
```

Por ejemplo:

```
"1024,-512,True,False\n"  
```

Añadir un salto de línea al final garantiza que el receptor detecte un paquete completo.

---

### 2. Formato del protocolo ASCII

La estructura establecida para cada mensaje es:

* **Separador de campos**: la coma `,`
* **Final de paquete**: el carácter `\n`
* **Orden fijo**: `xValue,yValue,aState,bState`
* **Tipos**:

  * `xValue`, `yValue`: enteros (valores del acelerómetro)
  * `aState`, `bState`: cadenas `"True"` o `"False"`

Este formato facilita el análisis en cualquier plataforma que lea líneas de texto, evitando ambigüedades.

---

### 3. Captura y conversión en p5.js

Dentro de la función `draw()` de tu sketch, compruebas si hay datos pendientes y procesas cada línea completa:

```javascript
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");   // Lee hasta el fin de línea
  if (data) {
    data = data.trim();               // Elimina espacios o '\r'
    let values = data.split(",");     // Divide en 4 partes
    if (values.length == 4) {
      // Ajuste de coordenadas al centro de la ventana
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      // Conversión de texto a booleano
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    }
  }
}
```

> **Tip adicional:** Si `values.length` no es 4, conviene registrar un error o descartar ese paquete, para evitar impactos en la lógica de tu aplicación.

---

### 4. Detección de eventos de botón

La función `updateButtonStates()` compara estados nuevos y anteriores para disparar eventos únicamente en los cambios reales:

```javascript
function updateButtonStates(newAState, newBState) {
  // Detecta la transición de A de falso a verdadero
  if (newAState === true && prevmicroBitAState === false) {
    lineModuleSize = random(50, 160);
    clickPosX = microBitX;
    clickPosY = microBitY;
    print("A pressed");
  }

  // Detecta la transición de B de verdadero a falso
  if (newBState === false && prevmicroBitBState === true) {
    c = color(random(255), random(255), random(255), random(80, 100));
    print("B released");
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}
```

Este método evita rebotes lógicos y garantiza que cada pulsación o liberación se maneje una sola vez.

---

### Conclusión

* El **micro\:bit** envía sus sensores y botones en un sencillo esquema CSV terminado en `\n`, perfecto para lectura por cualquier programa.
* En **p5.js**, usar `.trim()`, `.split()` e `int()` agiliza la transformación de esos datos en coordenadas y booleanos.
* Ajustar las coordenadas para centrar el dibujo y procesar sólo paquetes válidos mejora la experiencia visual.
* La detección de eventos por transición de estado es más fiable que leer el estado actual sin comparación.
