#### Actividad 10

``` js
let squareColor;
let changeColorBtn;

function setup() {
  createCanvas(400, 400);
  // Color inicial del cuadrado
  squareColor = color(255, 0, 0); // rojo
  // Crear el botón y asignarle la función de cambio de color
  changeColorBtn = createButton('Cambiar Color');
  changeColorBtn.position(150, 420);
  changeColorBtn.mousePressed(changeSquareColor);
}

function draw() {
  background(220);
  fill(squareColor);
  rectMode(CENTER);
  // Dibujar el cuadrado en el centro, simulando la pantalla del micro:bit
  rect(width / 2, height / 2, 100, 100);
}

function changeSquareColor() {
  // Genera un nuevo color aleatorio
  squareColor = color(random(255), random(255), random(255));
}

```
