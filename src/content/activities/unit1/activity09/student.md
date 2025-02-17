#### Actividad 9


``` js
function setup() {
  createCanvas(400, 400);
  background(30);
}

function draw() {
 
  let x = width / 2 + sin(frameCount * 0.1) * random(50, 150);
  let y = height / 2 + cos(frameCount * 0.1) * random(50, 150);
  
  let size = random(10, 40);
  
  let r = random(100, 255);
  let g = random(100, 255);
  let b = random(100, 255);
  
  noStroke();
  fill(r, g, b, 150);
  
  ellipse(x, y, size, size);
}

```
