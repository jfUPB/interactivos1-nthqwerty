#### Actividad 7

``` js
let STATE_CONFIG = 0;
let STATE_COUNTDOWN = 1;
let STATE_EXPLODED = 2;

let state = STATE_CONFIG;
let initial_time = 20; // en segundos
let countdownStart = 0;
let remaining = 0;

let btnUp, btnDown, btnShake, btnTouch;

function setup() {
  createCanvas(400, 400);
  
  btnUp = createButton('Botón A (UP)');
  btnUp.position(20, 420);
  btnUp.mousePressed(increaseTime);
  
  btnDown = createButton('Botón B (DOWN)');
  btnDown.position(150, 420);
  btnDown.mousePressed(decreaseTime);
  
  btnShake = createButton('Shake (ARMED)');
  btnShake.position(280, 420);
  btnShake.mousePressed(armBomb);
  
  btnTouch = createButton('Touch (Desactivar)');
  btnTouch.position(20, 460);
  btnTouch.mousePressed(touchBomb);
}

function draw() {
  background(220);
  textAlign(CENTER, CENTER);
  textSize(32);
  
  if (state === STATE_CONFIG) {
    text("Config: " + initial_time + " s", width / 2, height / 2);
  } 
  else if (state === STATE_COUNTDOWN) {
    let elapsed = floor((millis() - countdownStart) / 1000);
    remaining = initial_time - elapsed;
    if (remaining < 0) {
      remaining = 0;
    }
    text("Tiempo: " + remaining + " s", width / 2, height / 2);
    if (remaining === 0) {
      state = STATE_EXPLODED;
      // Aquí se podría reproducir un sonido de explosión usando la librería p5.sound
    }
  } 
  else if (state === STATE_EXPLODED) {
    text("BOOM!", width / 2, height / 2);
  }
}

function increaseTime() {
  if (state === STATE_CONFIG && initial_time < 60) {
    initial_time++;
  }
}

function decreaseTime() {
  if (state === STATE_CONFIG && initial_time > 10) {
    initial_time--;
  }
}

function armBomb() {
  if (state === STATE_CONFIG) {
    state = STATE_COUNTDOWN;
    countdownStart = millis();
  }
}

function touchBomb() {
  if (state === STATE_COUNTDOWN || state === STATE_EXPLODED) {
    state = STATE_CONFIG;
    initial_time = 20;
  }
}
```
