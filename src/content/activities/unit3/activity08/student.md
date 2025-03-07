#### Actividad 8

``` js
let STATE_CONFIG = 0;
let STATE_COUNTDOWN = 1;
let STATE_EXPLODED = 2;

let state = STATE_CONFIG;
let initial_time = 20; // Tiempo inicial en segundos
let countdownStart = 0;
let remaining = 0;

let event_occurred = false;
let event_value = null;

let port;
let connectBtn;
let btnUp, btnDown, btnArm, btnDeactivate;

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(32);
  
  // Inicializar comunicación serial
  port = createSerial();
  
  // Botón para conectar el puerto serial
  connectBtn = createButton('Conectar a micro:bit');
  connectBtn.position(20, 420);
  connectBtn.mousePressed(connectPort);
  
  // Botones para controlar la bomba desde p5.js
  btnUp = createButton('A (UP)');
  btnUp.position(20, 460);
  btnUp.mousePressed(() => setEvent('A'));
  
  btnDown = createButton('B (DOWN)');
  btnDown.position(120, 460);
  btnDown.mousePressed(() => setEvent('B'));
  
  btnArm = createButton('Shake (ARM)');
  btnArm.position(220, 460);
  btnArm.mousePressed(() => setEvent('S'));
  
  btnDeactivate = createButton('Touch (DEACTIVATE)');
  btnDeactivate.position(320, 460);
  btnDeactivate.mousePressed(() => setEvent('T'));
}

function draw() {
  background(220);
  tareaBomba();
  tareaEventos();
}

function connectPort() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
  } else {
    port.close();
  }
}

function setEvent(ev) {
  event_value = ev;
  event_occurred = true;
}

function tareaEventos() {
  // Revisar eventos del puerto serial si el puerto está abierto
  if (port && port.opened() && port.availableBytes() > 0 && !event_occurred) {
    let c = port.read(1);
    if (c) {
      let ch = c;
      // Convertir a carácter si es necesario
      if (typeof c === 'object' && c instanceof ArrayBuffer) {
        ch = String.fromCharCode(new Uint8Array(c)[0]);
      } else if (typeof c === 'string') {
        ch = c;
      }
      if (['A', 'B', 'S', 'T'].indexOf(ch) !== -1) {
        event_value = ch;
        event_occurred = true;
      }
    }
  }
}

function tareaBomba() {
  if (state === STATE_CONFIG) {
    text("Config: " + initial_time + " s", width / 2, height / 2);
    if (event_occurred) {
      if (event_value === 'A' && initial_time < 60) {
        initial_time++;
      } else if (event_value === 'B' && initial_time > 10) {
        initial_time--;
      } else if (event_value === 'S') {
        state = STATE_COUNTDOWN;
        countdownStart = millis();
      }
      event_occurred = false;
      event_value = null;
    }
  } else if (state === STATE_COUNTDOWN) {
    let elapsed = floor((millis() - countdownStart) / 1000);
    remaining = initial_time - elapsed;
    if (remaining < 0) remaining = 0;
    text("Time: " + remaining + " s", width / 2, height / 2);
    if (event_occurred) {
      if (event_value === 'T') {
        state = STATE_CONFIG;
        initial_time = 20;
      }
      event_occurred = false;
      event_value = null;
    }
    if (remaining === 0) {
      state = STATE_EXPLODED;
      // Aquí se puede reproducir un sonido de explosión usando p5.sound
    }
  } else if (state === STATE_EXPLODED) {
    text("BOOM!", width / 2, height / 2);
    if (event_occurred && event_value === 'T') {
      state = STATE_CONFIG;
      initial_time = 20;
    }
    event_occurred = false;
    event_value = null;
  }
}
```
