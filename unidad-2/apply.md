# Unidad 2


## 🛠 Fase: Apply

### Describe el concepto de tu obra generativa.
una estela de energía blanca golpea estrellas con baja temperatura para moverlas y aumentar su temperatura, en las estrellas el color nos dice su temperatura, mientras mas azul mas caliente y mientras mas roja más fria
<img width="555" height="147" alt="image" src="https://github.com/user-attachments/assets/0ff3bee0-3839-440f-adc9-f841e3b2793f" />

### ¿Cómo piensas aplicar el marco MOTION 101 y por qué?
Lo busco aplicar con el seguimiento al ratón (estela blanca) para que haya movimiento constante en dirección al raton y uno constante con desaceleración en las estrellas
### ¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?
aceleración hacia el mouse: sirve para que haya un poco de control en la obra
aceleración constante: la necesito para que el movimiento de las estrellas sea consistente
### El contenido generado debe ser interactivo. Puedes utilizar mouse, teclado, cámara, micrófono, etc, para variar los parámetros del algoritmo en tiempo real.
se usa el mouse, la esfera blanca sigue al mouse y "choca" con las estrellas para que se muevan
### El código de la aplicación.
```js
let mover;
let pelotas = [];
let numPelotas = 10;

function setup() {
  createCanvas(600, 400);
  background(0)
  mover = new Mover();

  for (let i = 0; i < numPelotas; i++) {
    pelotas.push(new Pelota());
  }
}

function draw() {
  

  mover.update();
  mover.dibujar();
  mover.checkEdges();

  for (let pelota of pelotas) {
    mover.checkCollision(pelota);
    pelota.update();
    pelota.dibujar();
    pelota.checkEdges();
  }
}

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0); 
  }

  update() {
    let mouse = createVector(mouseX, mouseY);
    this.acceleration = p5.Vector.sub(mouse, this.position);
    this.acceleration.setMag(0.09);
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
  }

  dibujar() {
    fill(255, 255, 255);
    noStroke();
    ellipse(this.position.x, this.position.y, 4, 4);
  }

  checkEdges() {
    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1;
    }
    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1;
    }
  }

  checkCollision(pelota) {
    let distancia = p5.Vector.dist(this.position, pelota.position);
    let radioSuma = 32;

    if (distancia < radioSuma) {
      // En lugar de copiar toda la velocidad, usamos su dirección
      let direccion = this.velocity.copy();
      if (direccion.mag() > 0) {
        direccion.normalize();
        direccion.mult(5); // Magnitud de impulso
        pelota.choque(direccion);
      }
    }
  }
}

class Pelota {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(0, 0);

    // Colores
    this.colorAzulOscuro = color(0, 0, 100);
    this.colorRojo = color(255, 0, 0);
    
    // Variables para interpolación suave
    this.targetAmt = 0;
    this.currentAmt = 0; 
    this.colorActual = this.colorAzulOscuro;
  }

  update() {
    this.position.add(this.velocity);
    this.velocity.mult(0.98);

    if (this.velocity.mag() < 0.01) {
      this.velocity.setMag(0);
    }

    
    let velocidadActual = this.velocity.mag();
    let velocidadMax = 4;
    this.targetAmt = constrain(map(velocidadActual, 0, velocidadMax, 0, 1), 0, 1);

    
    this.currentAmt = lerp(this.currentAmt, this.targetAmt, 0.1); 

    
    this.colorActual = lerpColor(this.colorAzulOscuro, this.colorRojo, this.currentAmt);
  }

  choque(impulso) {
    this.velocity = impulso.copy();
  }

  dibujar() {
    
    noStroke();
    fill(this.colorActual);
    ellipse(this.position.x, this.position.y, 32, 32);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = 0;
    }
    else if(this.position.x <= 0){this.position.x=width;}
    
    if (this.position.y > height) {this.position.y = 0;}
    else if(this.position.y <= 0){this.position.y=height;}
    
  }
}

```
### Un enlace al proyecto en el editor de p5.js.
https://editor.p5js.org/nijesa/full/_GNAhso46
### Una captura de pantalla representativa de tu pieza de arte generativo.
<img width="763" height="497" alt="image" src="https://github.com/user-attachments/assets/0446e6d6-b8a5-4e14-8a9b-eafb06138450" />
