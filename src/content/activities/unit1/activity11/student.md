#### Actividad 11

``` js
let circleX;
let buttonA;
let buttonB;

function setup() {
  createCanvas(400, 400);
  circleX = width / 2;
  buttonA = createButton('Boton A');
  buttonA.position(100, 420);
  buttonA.mousePressed(moveLeft);
  buttonB = createButton('Boton B');
  buttonB.position(200, 420);
  buttonB.mousePressed(moveRight);
}

function draw() {
  background(220);
  fill(0, 150, 255);
  ellipse(circleX, height / 2, 50, 50);
}

function moveLeft() {
  circleX -= 10;
  if (circleX < 0) {
    circleX = 0;
  }
}

function moveRight() {
  circleX += 10;
  if (circleX > width) {
    circleX = width;
  }
}

```
