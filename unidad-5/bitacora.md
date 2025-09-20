<img width="920" height="666" alt="image" src="https://github.com/user-attachments/assets/80c89f63-78e2-48fe-9112-d9ed4ac4403e" /># Evidencias de la unidad 5

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

3.Para este caso use las ondas y el movimiento sinoidal para las particulas, al empezar salen ondas de las 4 esquinas del canvas en forma de onda, van desapareciendo mientras se termina el lifespan, antes de explicar donde se usa el movimiento sinoidal hay que saber que hace un circulo en la mitad del canva, el circulo se puede mover con las flechas y su proposito es cambiar el movimiento de cualquier particula que lo toque, al chocar con el circulo la particula se mueve en la dirección opuesta con un movimiento sinoidal que recuerda un zigzag, la idea de este canva es ver como cambia el movimiento de las ondas cuando se chocan con un objeto, se usa el movimiento sinoidal para darle mas movimiento porque me parecia aburrido como se movian las particulas sin este efecto
4. [Enlace](https://editor.p5js.org/nijesa/sketches/Y50yb6OYK)
5.
```js
let systems = [];
let player;
let lastSpawnTime = 0;

function setup() {
  createCanvas(600, 400);


  player = new Player(width / 2, height / 2, 30);

  
  systems.push(new ParticleSystem(createVector(0, 0)));
  systems.push(new ParticleSystem(createVector(width, 0)));
  systems.push(new ParticleSystem(createVector(0, height)));
  systems.push(new ParticleSystem(createVector(width, height)));
}

function draw() {
  background(0,90);


  player.update();
  player.display();

  
  if (millis() - lastSpawnTime > 1000) {
    for (let s of systems) {
      for (let i = 0; i < 50; i++) {
        s.addParticle();
        s.addParticle();
        s.addParticle();
      }
    }
    lastSpawnTime = millis();
  }

  
  for (let s of systems) {
    s.run(player);
  }
}

class Player {
  constructor(x, y, r) {
    this.pos = createVector(x, y);
    this.r = r;
    this.speed = 4;
  }

  update() {
    if (keyIsDown(LEFT_ARROW)) this.pos.x -= this.speed;
    if (keyIsDown(RIGHT_ARROW)) this.pos.x += this.speed;
    if (keyIsDown(UP_ARROW)) this.pos.y -= this.speed;
    if (keyIsDown(DOWN_ARROW)) this.pos.y += this.speed;


    this.pos.x = constrain(this.pos.x, this.r / 2, width - this.r / 2);
    this.pos.y = constrain(this.pos.y, this.r / 2, height - this.r / 2);
  }

  display() {
    fill(0, 200, 255);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.r);
  }
}


class Particle {
  constructor(origin) {
    this.pos = origin.copy();
    let angle = random(TWO_PI);
    let speed = random(2, 2.2);
    this.vel = p5.Vector.fromAngle(angle).mult(speed);
    this.acc = createVector(0, 0);
    this.lifespan = 255;
    this.zigzag = false;
    this.offset = random(TWO_PI);
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);


    if (this.zigzag) {
      this.pos.y += sin(frameCount * 0.2 + this.offset) * 2;
    }

    this.lifespan -= 0.7;
  }

  display() {
    noStroke();
    fill(200, 150, 255, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 8);
  }

  isDead() {
    return this.lifespan < 0;
  }

  checkCollision(player) {
    let d = dist(this.pos.x, this.pos.y, player.pos.x, player.pos.y);
    if (d < player.r / 2) {
      this.vel.x *= -1;
      this.vel.y *= -1;
      this.zigzag = true;
    }
  }
}


class ParticleSystem {
  constructor(origin) {
    this.origin = origin.copy();
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin));
  }

  run(player) {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.checkCollision(player);
      p.display();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```
6. <img width="755" height="495" alt="image" src="https://github.com/user-attachments/assets/d8e18693-c1e8-41c7-9f17-bcaec47051a0" />





## Aplicación
1. ![BocetoAU5](https://github.com/user-attachments/assets/1e394f59-ebf0-42b7-b140-2477ab760c95)
    #### Concepto
   la idea es hacer una "colonia" de organismos que migran e interactuan entre si, serian puntos con colitas que salen de sistemas de particulas, interactuan atrayendose y repeliendose presentando movimientos armonicos cuando el usuario mete la mano. Usaria fuerzas locales y globales como viento, atracción y repulsion para generar patrones
   Busco comunicar el comportamiento de comunidades agresivas dentro de la misma especie, por ejemplo los peces beta que solo se atraen a hembras de su misma especie (o machos que parezcan hembras) mientras que atacan a los de su mismo genero o huyen de ellos.

2. Uso los concepto de herencia y polimorfismo con los organismos con cola y sin cola, viniendo ambos de la clase particle (que está vacia) y al heredar cambian entre ellas para parecer cosas diferentes
3. Uso vectores para la posición, velocidad y las fuerzas
   El motion 101 para darle posición, velocidad y aceleración
   Uso fuerzas como gravedad, atracción, repulsión y viento
   Seno y oscilar: los uso para darle sincronia, la onda senoidal atraviesa el canvas y las coordenadas polares para el spawn de las particulas
4. gestiono el tiempo de vida con el lifespan, que llevo usando toda la unidad, que al llegar a 0 reutiliza la particula y evita sobrecargas en la memoria, además limpio arrays iterando desde el final para poder hacer un splice(i,1).
6. [Enlace](https://editor.p5js.org/nijesa/sketches/CvcBiH8hy)
7.
```js
// Resonancia Migratoria
// Obra generativa con herencia/polimorfismo, motion101, fuerzas, sinusoides y pooling.

// ---------- Config / global ----------
let system;
let pools = { spark: [], ribbon: [] }; // object pools
let maxParticles = 600;
let showDebug = false;
let wave = { active: false, r:0, speed:6 };

function setup(){
  createCanvas(900, 600);
  colorMode(HSB, 360, 100, 100, 1);
  system = new ParticleSystem();

  // create some initial anchors (islas)
  for(let i=0;i<4;i++){
    let ang = map(i,0,4, -PI/6, PI + PI/6);
    let r = width*0.18;
    let pos = p5.Vector.fromAngle(ang).mult(r).add(createVector(width/2, height/2));
    system.addAnchor(pos);
  }

  // seeding initial particles
  for(let i=0;i<80;i++){
    system.spawnRandom();
  }

  textFont('Helvetica', 12);
}

function draw(){
  background(220, 20, 10, 0.12);

  // Draw central subtle gradient
  noStroke();
  for(let i=0;i<6;i++){
    fill(210, 10, map(i,0,5,2,10));
    ellipse(width/2, height/2, 800 - i*80, 800 - i*80);
  }

  // Interactivity UI (top)
  push();
  noStroke();
  fill(0,0,100,0.9);
  rect(0,0,width,26);
  fill(0,0,10);
  text('Move mouse = attractor | SHIFT+m = repulse | Click = pulse wave | 1 spark | 2 ribbon | s toggle sync | r reseed', 10, 18);
  pop();

  // active wave expands
  if(wave.active){
    wave.r += wave.speed;
    let a = map(wave.r, 0, max(width,height), 1, 0);
    stroke(200, 80, 80, a);
    strokeWeight(2);
    noFill();
    ellipse(mouseX, mouseY, wave.r*2);
    if(a <= 0.02) wave.active = false;
  }

  // compute attractor/repulsor force from mouse
  let attractor = createVector(mouseX, mouseY);
  let attractStrength = keyIsDown(SHIFT)? -900 : 800; // repulse if shift

  // evolve system: anchors produce particles and particles interact (pairwise small influence)
  system.run(attractor, attractStrength);

  if(showDebug){
    fill(0,0,100);
    noStroke();
    text('particles: ' + system.particles.length + ' poolS:' + pools.spark.length + ' poolR:' + pools.ribbon.length, 10, height-10);
  }
}

function keyPressed(){
  if(key === '1') for(let i=0;i<30;i++) system.spawnType('spark');
  if(key === '2') for(let i=0;i<15;i++) system.spawnType('ribbon');
  if(key === 'r' || key === 'R') {
    system.reset();
  }
  if(key === 's' || key === 'S') system.toggleSync();
}

function mousePressed(){
  // pulse wave
  wave.active = true;
  wave.r = 10;
  wave.speed = 8;

  // spawn local burst
  for(let i=0;i<12;i++){
    system.spawnBurst(createVector(mouseX, mouseY));
  }
}

// ---------- ParticleSystem, Anchors ----------
class ParticleSystem {
  constructor(){
    this.particles = [];
    this.anchors = []; // anchor points with slight oscillation
    this.sync = false; // global sync toggle
    this.syncPhase = 0;
  }

  addAnchor(pos){
    this.anchors.push({
      pos: pos.copy(),
      base: pos.copy(),
      amp: random(8,30),
      freq: random(0.005, 0.02),
      hue: random(180, 300)
    });
  }

  spawnRandom(){
    let a = random(this.anchors);
    let start = p5.Vector.add(a.pos, p5.Vector.random2D().mult(random(6,18)));
    this.spawnType(random(['spark','ribbon']));
  }

  spawnBurst(center){
    for(let i=0;i<18;i++){
      let t = random(['spark','ribbon']);
      let angle = random(TWO_PI);
      let r = random(6, 40);
      let ppos = createVector(center.x + cos(angle)*r, center.y + sin(angle)*r);
      this._spawn(t, ppos);
    }
  }

  spawnType(t){
    // spawn near random anchor using polar coordinates
    let a = random(this.anchors);
    let theta = random(-PI, PI);
    let r = random(10, 120);
    let spawnPos = p5.Vector.fromAngle(theta).mult(r).add(a.pos);
    this._spawn(t, spawnPos);
  }

  _spawn(type, pos){
    if(this.particles.length >= maxParticles) return;
    let p;
    // pooling
    if(type === 'spark'){
      if(pools.spark.length > 0) p = pools.spark.pop();
      else p = new Spark();
    } else {
      if(pools.ribbon.length > 0) p = pools.ribbon.pop();
      else p = new Ribbon();
    }
    p.onSpawn(pos);
    this.particles.push(p);
  }

  toggleSync(){ this.sync = !this.sync; }

  reset(){
    // move all active to pool
    for(let p of this.particles){
      p.reset();
      if(p instanceof Spark) pools.spark.push(p);
      else pools.ribbon.push(p);
    }
    this.particles = [];
    // re-randomize anchors
    this.anchors = [];
    for(let i=0;i<4;i++){
      let ang = map(i,0,4, -PI/6, PI + PI/6);
      let r = width*0.18;
      let pos = p5.Vector.fromAngle(ang).mult(r).add(createVector(width/2, height/2));
      this.addAnchor(pos);
    }
  }

  run(attractor, strength){
    // animate anchors (sinusoidal movement, UNIT: sinusoids)
    for(let a of this.anchors){
      let ang = frameCount * a.freq;
      a.pos.x = a.base.x + cos(ang) * a.amp;
      a.pos.y = a.base.y + sin(ang * 1.1) * (a.amp*0.4);
      // draw anchor
      noStroke();
      fill(a.hue, 60, 60, 0.9);
      ellipse(a.pos.x, a.pos.y, 10);
    }

    // spawn a few each frame near anchors
    for(let i=0;i<2;i++) this.spawnType(random(['spark','ribbon']));

    // optional global sync: tiny phase nudges to each particle (coupling)
    if(this.sync){
      this.syncPhase += 0.02;
    }

    // apply interactions and update particles
    // also simple pairwise short-range attraction among particles (but limited)
    for(let i=this.particles.length -1; i>=0; i--){
      let p = this.particles[i];

      // attractor from mouse / repulse
      let dir = p5.Vector.sub(attractor, p.pos);
      let d = dir.mag();
      d = constrain(d, 5, 500);
      dir.normalize();
      let fmag = (strength * p.mass) / (d*d); // inverse-square-ish
      // clamp magnitude to avoid explosion
      fmag = constrain(fmag, -120, 120);
      let f = dir.mult(fmag);
      p.applyForce(f);

      // wave perturbation: if wave active and particle near wave radius from mouse, apply lateral sine kick
      if(wave.active){
        let distToWaveCenter = p5.Vector.dist(p.pos, createVector(mouseX, mouseY));
        if(abs(distToWaveCenter - wave.r) < 32){
          // lateral push using perpendicular direction and sinusoidal modulation
          let perp = createVector(-(p.pos.y - mouseY), p.pos.x - mouseX).normalize();
          let kick = perp.mult(3 * sin((frameCount*0.2 + p.id)*0.8));
          p.applyForce(kick);
        }
      }

      // local pairwise weak attraction/repulsion to create flocking-like group (limited cost)
      // sample a few neighbors randomly to approximate N^2 cheaply
      for(let k=0;k<3;k++){
        let j = floor(random(this.particles.length));
        if(j===i) continue;
        let q = this.particles[j];
        let dv = p5.Vector.sub(q.pos, p.pos);
        let dd = dv.mag();
        if(dd < 40 && dd > 2){
          // slight repulsion to avoid overlap
          let rep = dv.copy().normalize().mult(-0.02 * (40-dd));
          p.applyForce(rep);
        } else if(dd < 140 && dd > 40){
          // mild attraction
          let att = dv.copy().normalize().mult(0.002 * (dd-40));
          p.applyForce(att);
        }
      }

      // self update
      // if sync on, apply sinusoids to angle/oscillator of particle subclasses
      if(this.sync) p.applySync(this.syncPhase);

      p.update();
      p.display();

      if(p.isDead()){
        // recycle to pool
        this.particles.splice(i,1);
        p.reset();
        if(p instanceof Spark) pools.spark.push(p);
        else pools.ribbon.push(p);
      }
    }
  }
}

// ---------- Base Particle ----------
let _PARTICLE_ID = 0;
class Particle {
  constructor(){
    this.id = _PARTICLE_ID++;
    this.pos = createVector();
    this.vel = createVector();
    this.acc = createVector();
    this.mass = 1;
    this.lifespan = 1.0; // normalized 1..0
    this.lifeDecay = 0.005 + random(0.0005, 0.002);
  }

  onSpawn(pos){
    // initialize
    this.pos = pos.copy();
    this.vel = p5.Vector.random2D().mult(random(0.2,1.6));
    this.acc = createVector(0,0);
    this.mass = random(0.6, 1.6);
    this.lifespan = 1.0;
    this.lifeDecay = 0.001 + random(0.001, 0.004);
  }

  applyForce(f){
    let ff = p5.Vector.div(f, this.mass);
    this.acc.add(ff);
  }

  applySync(phase){
    // base does nothing; subclasses may override to accept a phase parameter (polymorphism)
  }

  update(){
    // Motion 101
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.vel.mult(0.995); // small damping
    this.lifespan -= this.lifeDecay;
  }

  display(){
    // placeholder: subclasses override
    noStroke();
    fill(0,0,100,this.lifespan);
    ellipse(this.pos.x, this.pos.y, 4);
  }

  isDead(){
    return this.lifespan <= 0;
  }

  reset(){
    // clear heavy structures if any (for pooling safety)
    this.acc.set(0,0);
    this.vel.set(0,0);
  }
}

// ---------- Spark (pointy) ----------
class Spark extends Particle {
  constructor(){ super(); this.hue = random(0,360); this.size = random(2,6); this.osc = random(0.1,1.0);}
  onSpawn(pos){
    super.onSpawn(pos);
    this.hue = random(160, 320);
    this.size = random(2,6);
    this.osc = random(2,8);
  }
  applySync(phase){
    // change velocity slightly according to a sinusoidal phase (coupling)
    let s = sin(phase + this.id*0.07) * 0.05;
    this.applyForce(createVector(s, -s));
  }
  update(){
    // small SHM on size using internal oscillator
    this.size = 2 + 4 * (0.5 + 0.5*sin(frameCount * 0.02 * this.osc + this.id));
    super.update();
  }
  display(){
    noStroke();
    let alpha = constrain(this.lifespan, 0, 1);
    fill(this.hue, 70, 70, alpha);
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}

// ---------- Ribbon (trail) ----------
class Ribbon extends Particle {
  constructor(){ super(); this.hue = random(0,360); this.len = 18; this.history = []; this.maxH=30; }
  onSpawn(pos){
    super.onSpawn(pos);
    this.hue = random(100, 220);
    this.len = random(12,28);
    this.history = [];
    this.maxH = 24;
  }
  applySync(phase){
    // slight torque on velocity rotating it, to create swirling synchronized motion
    let ang = 0.02 * sin(phase + this.id*0.03);
    this.vel.rotate(ang);
  }
  update(){
    // record history for tail
    this.history.push(this.pos.copy());
    if(this.history.length > this.maxH) this.history.shift();
    super.update();
  }
  display(){
    // draw tail as polyline fading with age
    noFill();
    stroke(this.hue, 70, 70, 0.9);
    strokeWeight(2);
    beginShape();
    for(let i=0;i<this.history.length;i++){
      let h = this.history[i];
      let t = i/this.history.length;
      stroke(this.hue, 70, 70, t*0.9);
      vertex(h.x, h.y);
    }
    endShape();
    // head
    noStroke();
    fill(this.hue, 70, 70, 1.0);
    ellipse(this.pos.x, this.pos.y, 5 + this.mass*3);
  }
}

```
8. <img width="920" height="666" alt="image" src="https://github.com/user-attachments/assets/490a3b1b-88dd-4d9b-bfbf-3aa71d9c0cac" />
