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
3. 
4. 
5. 
6. 
