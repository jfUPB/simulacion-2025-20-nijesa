# Evidencias de la unidad 5

## Actividad 2
### Ejemplo 4.2

2. Recorro el arreglo para ver el tiempo de vida de cada particula y llamar los metodos de la clase, cuando el tiempo de vida de una particula llega a ser menor que 0, uso [splice](https://p5js.org/reference/p5/splice/) para moverla, asi la CPU no gasta memoria en particulas que no se usan mas, en este caso porque salen del area visible y asi evitar que el arreglo crezca indefinidamente, al mover las particulas con splice reutilizamos las que salen del area de vision y evitamos una alta creacion de particulas, ya que si crece tanto el rendimiento del proyecto empezará a decaer lageando el proyecto

3.  El codigo busca asemejarse al comportamiento de una llama cuando la soplan o cuando es afectada por el viento, la direccion del viento se da con la posición del mouse en x desde el centro del canvas, uso la funcion addForce(force) para ejercer la fuerza en cada particula y así crear un efecto parecido al comportamiento de una llama

4. [Enlace](https://editor.p5js.org/nijesa/sketches/lTNqjc9hQ)

5. 
```js 
let particles = [];

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(51);

 
  particles.push(new Particle(createVector(width/2, height/2)));

 
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];

    
    let viento = createVector(map(mouseX, 0, width, -0.2, 0.2), 0);
    p.applyForce(viento);

    p.update();
    p.display();

    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }
}

class Particle {
  constructor(pos) {
    this.pos = pos.copy();
    this.vel = createVector(random(-1, 1), random(-2, 0));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;
  }

  display() {
    noStroke();
    fill(200, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 12);
  }

  isDead() {
    return this.lifespan < 0;
  }
}

```

6. <img width="577" height="346" alt="image" src="https://github.com/user-attachments/assets/a3136ac7-c15e-4a12-842a-a3c589de59c9" />


Ejercicio 4.4
3. El codigo busca replicar el comportamiento de cumulos de estrellas ocambiando de dirección a gran velocidad por la fuerza gravitacional de un objeto celeste, esto se logra poniendo un objeto que ejerza fuerza de gravedad en las particulas (en este caso estrellas) para cambiar su dirección
4. [Enlace](https://editor.p5js.org/nijesa/sketches/MxSIoPEXO)
5. 
```js
let particleSystems = []; 
let gravityCenter; 

function setup() {
  createCanvas(600, 400);
  gravityCenter = createVector(width / 2, height / 2); 
}

function draw() {
  background(51);


  fill(100, 150, 255);
  noStroke();
  ellipse(gravityCenter.x, gravityCenter.y, 40);


  for (let i = particleSystems.length - 1; i >= 0; i--) {
    let ps = particleSystems[i];
    ps.run();


    if (ps.isDead()) {
      particleSystems.splice(i, 1);
    }
  }
}

function mousePressed() {

  let newSystem = new ParticleSystem(createVector(mouseX, mouseY));

 
  if (particleSystems.length >= 5) {
    particleSystems.shift();
  }

  particleSystems.push(newSystem);
}


class ParticleSystem {
  constructor(position) {
    this.origin = position.copy();
    this.particles = [];
    this.lifespan = 52; 
  }

  run() {
    
    this.particles.push(new Particle(this.origin));

   
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];

      
      let dir = p5.Vector.sub(gravityCenter, p.pos);
      dir.setMag(0.2); 
      p.applyForce(dir);

      p.update();
      p.display();

     
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }

   
    this.lifespan--;
  }

  isDead() {
    return this.lifespan <= 0 && this.particles.length === 0;
  }
}


class Particle {
  constructor(pos) {
    this.pos = pos.copy();
    this.vel = createVector(random(-1, 1), random(-2, 0));
    this.acc = createVector(0, 0);
    this.lifespan = 360;
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2; 
  }

  display() {
    noStroke();
    fill(200, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 12);
  }

  isDead() {
    return this.lifespan < 1;
  }
}

```
6. <img width="747" height="502" alt="image" src="https://github.com/user-attachments/assets/f5b91aeb-5782-4357-830e-a4f96c38e2a6" />


### Ejemplo 4.5
3. El concepto de este codigo son fuegos artificiales, cuando se toca una tecla se añade una particula a las particulas que se generan, los colores cambian dependiendo del lifespan usando lerpcolor, osea aleatoriedad controlada, el efecto de fuegos artificiales se da desde las figuras grices que aparecen al principio (estas aparecen de manera aleatoria por el canvas) el reciclado de particulas es el mismo que he usado desde el principio
4. [Enlace](https://editor.p5js.org/nijesa/sketches/7gkmYbYnN)
5. 
```js
let particleSystems = []; 
let shapes = ["circle", "square"];

function setup() {
  createCanvas(600, 400);

  
  for (let i = 0; i < 3; i++) {
    let pos = createVector(random(width), random(height));
    let shapeType = random(shapes);
    particleSystems.push(new ParticleSystem(pos, shapeType));
  }
}

function draw() {
  background(51);


  for (let ps of particleSystems) {
    ps.run();
  }
}

function keyPressed() {
 
  for (let ps of particleSystems) {
    ps.addParticle();
  }
}


class Particle {
  constructor(origin) {
    this.origin = origin.copy(); // centro del sistema
    this.angle = random(TWO_PI); // ángulo de salida
    this.radius = 0;             // distancia al centro (expande como onda)
    this.lifespan = 255;
  }

  update() {
    
    this.radius += 2; 
    this.pos = p5.Vector.add(
      this.origin,
      createVector(cos(this.angle) * this.radius, sin(this.angle) * this.radius)
    );
    this.lifespan -= 4;
  }

  display() {
    let c1 = color(0, 200, 255);
    let c2 = color(255, 100, 0);
    let t = map(this.lifespan, 255, 0, 0, 1);
    let c = lerpColor(c1, c2, t);

    noStroke();
    fill(c, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 12);
  }

  isDead() {
    return this.lifespan < 0;
  }
}


class Confetti extends Particle {
  display() {
    let c1 = color(50, 255, 100);
    let c2 = color(200, 0, 200);
    let t = map(this.lifespan, 255, 0, 0, 1);
    let c = lerpColor(c1, c2, t);

    push();
    translate(this.pos.x, this.pos.y);
    rotate(frameCount / 20.0);
    fill(c, this.lifespan);
    rectMode(CENTER);
    rect(0, 0, 10, 10);
    pop();
  }
}


class ParticleSystem {
  constructor(pos, type) {
    this.origin = pos.copy();
    this.particles = [];
    this.type = type; 
  }

  addParticle() {
    if (this.type === "circle") {
      this.particles.push(new Particle(this.origin));
    } else {
      this.particles.push(new Confetti(this.origin));
    }
  }

  run() {
    
    noStroke();
    fill(255, 150); // marcador más sutil
    if (this.type === "circle") {
      ellipse(this.origin.x, this.origin.y, 20); 
    } else {
      rectMode(CENTER);
      rect(this.origin.x, this.origin.y, 20, 20); 
    }


    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.display();

      
      if (p.isDead()) {
        this.particles.splice(i, 1);
        this.addParticle();
      }
    }
  }
}

```
6. <img width="745" height="507" alt="image" src="https://github.com/user-attachments/assets/f243e3df-ac6a-4769-a9f6-fe06194858cb" />

### Ejemplo 4.6

3. El comportamiento que se muestra en el codigose busca que sea una ducha o cascada (representada por las particulas que caen) y un objeto que se mueve dentro del agua, empujandola y bloqueando su caida, mientras mas rapido se mueve el objeto mas se ven afectadas las particulas con las que choca, se aplicaron los conceptos de fuerzas y vectores, las colisiones se hicieron con una clase dentro de particle que toman pa posición del cuadrado +/- la mitad de su tamaño para saber con que lado estan colisionando, se le aplica un vector cuya magnitud está atada a suvelocidad y ya, si el cuadrado esta quieto las particulas se quedan encima y se mueven a uno de los lados del cuadrado
4. [Enlace](https://editor.p5js.org/nijesa/sketches/V4uVY4BjI)
5.
```js
let ps;
let prevMouse; 
let mouseVel;  

function setup() {
  createCanvas(600, 400);
  ps = new ParticleSystem(createVector(width / 4, 50));
  prevMouse = createVector(mouseX, mouseY);
  mouseVel = createVector(0, 0);
}

function draw() {
  background(51);

  
  mouseVel = createVector(mouseX - prevMouse.x, mouseY - prevMouse.y);
  prevMouse.set(mouseX, mouseY);

 
  fill(255, 150);
  noStroke();
  rectMode(CENTER);
  rect(mouseX, mouseY, 25, 25);

  // fuerzas globales
  let gravedad = createVector(0, 0.1);
  let viento = createVector(0.02, 0);

  ps.addParticle();
  ps.applyForce(gravedad);
  ps.applyForce(viento);

  ps.run();
}

class Particle {
  constructor(pos) {
    this.pos = pos.copy();
    this.vel = createVector(random(-1, 1), random(-2, 0));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
    this.r = 6; 
  }

  applyForce(f) { this.acc.add(f); }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;

   
    let half = 25 / 2;
    let left = mouseX - half;
    let right = mouseX + half;
    let top = mouseY - half;
    let bottom = mouseY + half;

    
    if (this.pos.x > left && this.pos.x < right &&
        this.pos.y > top && this.pos.y < bottom) {
      
     
      let overlapX = min(abs(this.pos.x - left), abs(this.pos.x - right));
      let overlapY = min(abs(this.pos.y - top), abs(this.pos.y - bottom));

      if (overlapX < overlapY) {
        
        if (this.pos.x < mouseX) {
          this.pos.x = left - this.r; 
        } else {
          this.pos.x = right + this.r; 
        }
        
        let force = createVector(mouseVel.x, 0);
        force.mult(0.2); 
        this.applyForce(force);
      } else {
        
        if (this.pos.y < mouseY) {
          this.pos.y = top - this.r; 
        } else {
          this.pos.y = bottom + this.r; 
        }
     
        let force = createVector(0, mouseVel.y);
        force.mult(0.2);
        this.applyForce(force);

        this.applyForce(createVector(random(-0.1, 0.1), 0));
      }
    }
  }

  display() {
    fill(0, 180, 255, this.lifespan);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.r * 2);
  }

  isDead() { return this.lifespan < 0; }
}

class ParticleSystem {
  constructor(pos) {
    this.origin = pos.copy();
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin));
  }

  applyForce(f) {
    for (let p of this.particles) p.applyForce(f);
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.display();
      if (p.isDead()) this.particles.splice(i, 1);
    }
  }
}

```
6. <img width="562" height="496" alt="image" src="https://github.com/user-attachments/assets/f605a286-bc3f-4fbb-8aa2-c801438caffa" />




### Ejemplo 4.7

3.e
4.e
5.
```js

```
6.e

