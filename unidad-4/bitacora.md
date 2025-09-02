# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
  De la unidad 4 usé el movimiento senoidal en el movimiento de caida de las hojas y las ondas que salen cuando choca con el piso
>

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
  se utiliza el motion 101 para la fuerza del viento que se aplica a las hojas en 
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
  El concepto de vectores se usa en el movimiento de las hojas, tienen un vector posicion, un vector velocidad y uno aceleración
>

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
  Uso el random para la posición de donde salen las hojas
>

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí:
  las flechas dictan la dirección del viento que se aplica a las hojas
>

## Enlace a la obra en el editor de p5.js

[[Aquí está mi obra](https://editor.p5js.org/nijesa/sketches/xrtrgNAhS)]

## Código de la obra 

``` js
let hojas = [];   
let ondas = [];   
let numHojas = 50; 

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < numHojas; i++) {
    hojas.push(new Hoja(random(width), random(-height, 0)));
  }
}

function draw() {
  background(200, 230, 255, 80);
if (keyIsDown(RIGHT_ARROW)) viento = 0.1;
  else if (keyIsDown(LEFT_ARROW)) viento = -0.1;
  else viento = 0;
  // dibujar y mover hojas
  for (let hoja of hojas) {
    hoja.mover(viento);
    hoja.mostrar();
  }

  
  for (let i = ondas.length - 1; i >= 0; i--) {
    ondas[i].update();
    ondas[i].show();

    if (ondas[i].alpha <= 0) {
      ondas.splice(i, 1); 
    }
  }
}


class Hoja {
  constructor(x, y) {
    this.pos=createVector(this.x,this.y);
    this.x = x;
    this.y = y;
    this.vel=createVector(0,random(1,3));
    this.accel=createVector(0,0);    
    this.tam = random(15, 30);
    this.color = color(random(100, 255), random(100, 200), 0);
    
  }

  mover(fuerzaViento) {
    
    this.y += this.vel.y;
    this.vel.x+=fuerzaViento;
    this.vel.x*=0.95;
    this.x += this.vel.x + sin(this.y * 0.05);

    if (this.y > height) {
      ondas.push(new Onda(this.x, height)); 
      this.y = random(-50, 0);
      this.x = random(width);
      this.vel.x = 0; 
    }
  }

  mostrar() {
    push();
    translate(this.x, this.y);
    noStroke();
    fill(this.color);
    ellipse(0, 0, this.tam, this.tam * 0.6);
    pop();
  }
}


class Onda {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.radio = 5;        
    this.vel = 4;          
    this.alpha = 255;      
  }

  update() {
    this.radio += this.vel;
    this.alpha = map(this.y - this.radio, 0, height, 0, 255); 
    this.alpha = constrain(this.alpha, 0, 255);
  }

  show() {
    noFill();
    stroke(0, 100, 200, this.alpha);
    strokeWeight(2);
    ellipse(this.x, this.y, this.radio * 2);
  }
}
```

## Captura de pantalla representativa
<img width="745" height="501" alt="image" src="https://github.com/user-attachments/assets/d5ffbf03-5b0a-41e9-9aba-a484578b8245" />









