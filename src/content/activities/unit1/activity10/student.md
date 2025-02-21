#### Actividad 10

``` js
let squareColor;
let changeColorBtn;

function setup() {
  createCanvas(400, 400);
  squareColor = color(255, 0, 0);
  changeColorBtn = createButton('Cambiar Color');
  changeColorBtn.position(150, 420);
  changeColorBtn.mousePressed(changeSquareColor);
}

function draw() {
  background(220);
  fill(squareColor);
  rectMode(CENTER);
  rect(width / 2, height / 2, 100, 100);
}

function changeSquareColor() {
  squareColor = color(random(255), random(255), random(255));
}

```
