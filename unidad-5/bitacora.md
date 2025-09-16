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


