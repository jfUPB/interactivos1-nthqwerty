#### Actividad 9


``` js
function setup() {
  createCanvas(600, 600);
  noLoop();
}

function draw() {
  background(20);
  noFill();
  
  let cols = 10;
  let rows = 10;
  let spacing = width / cols;

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      let x = i * spacing + spacing / 2;
      let y = j * spacing + spacing / 2;

      let radius = map(sin(frameCount * 0.05 + i * 0.3), -1, 1, 10, 50);
      let angleOffset = noise(i * 0.2, j * 0.2) * TWO_PI;

      let r = random(100, 255);
      let g = random(50, 200);
      let b = random(150, 255);

      stroke(r, g, b, 180);
      strokeWeight(2);

      drawPattern(x, y, radius, angleOffset);
    }
  }
}

function drawPattern(x, y, radius, angleOffset) {
  beginShape();
  for (let a = 0; a < TWO_PI; a += PI / 6) {
    let offset = sin(a * 3 + angleOffset) * 10;
    let xOff = x + (radius + offset) * cos(a);
    let yOff = y + (radius + offset) * sin(a);
    vertex(xOff, yOff);
  }
  endShape(CLOSE);
}

function mousePressed() {
  redraw();
}
```
