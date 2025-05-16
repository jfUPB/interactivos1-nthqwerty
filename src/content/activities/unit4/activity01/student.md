#### Actividad 1

Ejemplo 1.

https://openprocessing.org/sketch/2591677

En esencia, el codigo presenta elementos sencillos, pero juntos crean un efecto muy interesante y llamativo, agrandando la letra del poema puesto de fondo para ilustrar una llama emergiendo del texto.

Este sketch en p5.js genera una visualización artística de un poema utilizando una cuadrícula de letras renderizadas en la pantalla. Se hace uso de funciones fundamentales de p5.js como preload() para cargar la fuente, setup() para definir el lienzo y establecer configuraciones iniciales, y draw() para renderizar continuamente el texto en función del ruido y el tiempo. El programa implementa técnicas de programación como el uso de arreglos para manipular el contenido del poema, estructuras de control anidadas para recorrer el lienzo con una cuadrícula y funciones como noise() y map() para generar variaciones visuales suaves en el tamaño del texto, lo que produce un efecto dinámico y orgánico. Además, se hace un uso eficiente del módulo % para recorrer el arreglo del poema repetidamente, lo que permite una animación continua del texto.


https://editor.p5js.org/nthqwerty/sketches/lGpEvtLe1

En el codigo modificado, cambie el color de fondo a uno mas amarillento simbolizando el fuego de la animacion, y cambie la fuente a Comic Sans.

---------------
Ejemplo 2.

https://editor.p5js.org/generative-design/sketches/P_2_2_1_01

En este código, un punto empieza en el centro de la pantalla y se va moviendo al azar en diferentes direcciones. Mientras se mueve, va dejando marcas pequeñas (como puntitos). Cuanto más mueves el mouse hacia la derecha, más rápido se mueve ese punto. Si llega al borde de la pantalla, aparece del otro lado, como si la pantalla fuera un mundo circular. Puedes borrar todo lo que ha dibujado presionando DEL o BACKSPACE, y guardar una imagen de lo que ves presionando la tecla s.

El sketch de p5.js utiliza principalmente las funciones setup() y draw() para crear una animación interactiva. En setup(), se configura el lienzo del tamaño de la ventana y se define el color del relleno de las figuras. En draw(), se ejecuta un bucle que mueve un punto desde el centro de la pantalla en una dirección aleatoria entre ocho posibles (norte, noreste, este, etc.), simulando un movimiento errático. El número de movimientos por fotograma depende de la posición del mouse en el eje X, haciendo que el usuario controle la velocidad del dibujo con el mouse. Cada paso deja un pequeño punto dibujado en la pantalla. También se usan eventos de teclado: al presionar s, se guarda una imagen del canvas, y con DELETE o BACKSPACE, se limpia la pantalla. El sketch aplica técnicas de animación simple y control interactivo, combinando entrada del usuario con lógica de movimiento aleatorio.

https://editor.p5js.org/nthqwerty/sketches/E0P8Pd0en

En la version modificada, hice que el trazo del pincel seas mas definido y que la velocidad de este no dependa del mouse y sea constantemente rapida.

---------------
Ejemplo 3.

https://p5js.org/examples/repetition-kaleidoscope/

En este ejemplo, al dar clic en la pantalla y mover el mouse, el programa crea un efecto de caleidoscopio, creando un patron interesante.

Este sketch en p5.js permite crear un dibujo interactivo con simetría rotacional, generando un efecto visual tipo mandala. El lienzo se divide en varias secciones iguales, determinadas por la variable symmetry, y cada línea trazada por el mouse se repite automáticamente en esas secciones mediante rotaciones con rotate, reflejos con scale, y el uso de push y pop para preservar el estado del lienzo. El origen de coordenadas se traslada al centro con translate para que las rotaciones ocurran desde ese punto. Solo cuando el mouse está dentro del área visible y presionado, se dibujan líneas entre la posición actual y la anterior del cursor, las cuales se repiten simétricamente. Se usan funciones clave como createCanvas para definir el área de dibujo, background para establecer el fondo, y angleMode(DEGREES) para facilitar las rotaciones. Este código combina de forma efectiva transformaciones gráficas y entrada del usuario para crear una experiencia artística interactiva.

https://editor.p5js.org/nthqwerty/sketches/fhSNBs6UE

En el nuevo codigo, cambie el trazo con el que se crea el efecto.



