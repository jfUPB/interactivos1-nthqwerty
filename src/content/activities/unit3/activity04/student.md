#### Actividad 4

``` js
let port;
let connectBtn;
let btnA, btnB, btnShake, btnTouch;

function setup() {
  createCanvas(400, 400);
  background(220);
  
  port = createSerial();
  
  connectBtn = createButton('Conectar a micro:bit');
  connectBtn.position(20, 420);
  connectBtn.mousePressed(connectPort);
  
  btnA = createButton('Botón A');
  btnA.position(20, 460);
  btnA.mousePressed(() => port.write('A'));
  
  btnB = createButton('Botón B');
  btnB.position(120, 460);
  btnB.mousePressed(() => port.write('B'));
  
  btnShake = createButton('Shake');
  btnShake.position(220, 460);
  btnShake.mousePressed(() => port.write('S'));
  
  btnTouch = createButton('Touch');
  btnTouch.position(320, 460);
  btnTouch.mousePressed(() => port.write('T'));
}

function draw() {
  background(220);
}

function connectPort() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
  } else {
    port.close();
  }
}

```
