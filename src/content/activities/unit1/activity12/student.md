####Actividad 12

``` js
from microbit import *

uart.init(baudrate=115200)
display.show(Image.SURPRISED)  # Imagen inicial (SURPRISED)

while True:
    if button_a.was_pressed():
        display.show(Image.CONFUSED)
        uart.write('A')
        sleep(500)
    if button_b.was_pressed():
        display.show(Image.ANGRY)
        uart.write('B')
        sleep(500)
    if accelerometer.was_gesture('shake'):
        display.show(Image.ASLEEP)
        uart.write('C')
        sleep(500)
    if uart.any():
        data = uart.read(1)
        if data and data[0] == ord('h'):
            display.show(Image.SURPRISED)
            sleep(500)

```

``` js
let port;
let connectBtn;
let sendLoveBtn;

function setup() {
  createCanvas(400, 400);
  background(220);
  
  port = createSerial();
  
  connectBtn = createButton('Conectar a micro:bit');
  connectBtn.position(20, 420);
  connectBtn.mousePressed(connectBtnClick);
  
  sendLoveBtn = createButton('Send Love');
  sendLoveBtn.position(200, 420);
  sendLoveBtn.mousePressed(sendLove);
}

function draw() {
  background(220);
  textAlign(CENTER, CENTER);
  textSize(16);
  fill(0);
  text("Usa los botones físicos del micro:bit:\n- Botón A: CONFUSED\n- Botón B: ANGRY\n- Sacude: ASLEEP\n\nO presiona 'Send Love' para SURPRISED", width / 2, height / 2);
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
  } else {
    port.close();
  }
}

function sendLove() {
  port.write('h');
}

```
