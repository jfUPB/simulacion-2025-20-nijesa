# Evidencias de la unidad 6

## Actividad 1
<img width="835" height="970" alt="image" src="https://github.com/user-attachments/assets/9308cf28-eca9-46eb-a43f-9befd7189681" />

me llama la atencion la forma en la que hizo el algoritmo para crear una pieza con 999 NFTs sin escoger nada a mano, solamente el algoritmo, lo que crea "automaticamente" es una pieza muy linda, aunque no sea fan de los NFTs (que no me llaman la atencion) me parece que usó los recursos de la mejor manera posible

<img width="1042" height="1041" alt="image" src="https://github.com/user-attachments/assets/a2d6130c-1efe-49ae-941d-c642d0da5103" />

Me interesó esta obra por lo simple que es, como dicen en el video solo usa un color y fondo blanco, me gusta que con dos cosas tan simples haya creado un codigo que logra hacer una pieza tan facinante

## Actividad 2

¿Qué es una fuerza de dirección (steering force)?
Es un vector que modifica el comportamiento de un agente para que cambie su rumbo o velocidad deseada, ajustando la dirección de manera mas inteligente, no solo usa las leyes del movimiento. En lugar de aplicar una fuerza constante la steering force calcula la diferencia entre lo que el agente quiere hacer y lo que está haciendo

¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?

- Fuerzas ya usadas: aplican un efecto directo en la aceleración del objeto, son independientes de un objeto explicito, solo obedecen leyes fisicas, transitan directamente desde el entorno y se combinan linealmente
- Steering Force: tiene logica de desición, no es una simple fuerza constante sino que se reevalu cada frame para adaptarse a las condiciones, buscan ajustar el rumbo, integran logica de comportamiento, están limitadas por la magnitud

¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?

craig usa la steering behaviour para simular un movimiento realista en animales, personajes, etc. Cada comportamiento devuelve un vector de steering y estos se combinan para generar nuevos resultados. Reynolds propone que el agente calcule su velocidad deseada y luego calcule la diferencia
la steering force permite que los agentes decidan su dirección de manera modular, mezclable, emergente e integrando logica


## Acividad 3
1. Estructura del campo de flujo: Este se implementa como una matriz 2D donde cada parte tiene un vector representando la dirección en ese punto, cada elemento de la matriz corresponde a una posición de la cuadricula en la pantalla y guarda un vector que nos dice a donde se debe mover el agente si se encuentra en esas región, estos vectores se calculan al inicio usando noise() para que el campo no sea caotico
2. Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
con follow, toma su posición actual y mapea indices de la cuadricula, con los indices creados busca en la matriz el vector de flujo correspondiente. Este vector es la velocidad deseada, para calcular la steering force el agente resta su velocidad a la velocidad deseada
3. Parametros clave
- Resolución del campo de flujo: define el tamaño de las celdas, valor bajo=celdas grandes y menos detalles, valor alto = celdas pequeñas, mayor detalle en el flujo
- maxspeed: velocidad maxima del agente
- maxforce: fuerza maxima de direción para ajustar el rumbo del agente cada frame
4. Modificación en el codigo
Reduje la resolción del flowfield lo que hizo que el campo fuera mas deallado, haciendo el movimiento mas fluido, lo que hace que los agentes se adaptan constantemente a cambios de dirección más frecuentes y pequeños en la cuadrícula, el movimiento de los agentes es caotico y turbulento, pero la forma en la que se mueven es ma natural
<img width="646" height="249" alt="image" src="https://github.com/user-attachments/assets/54a471c3-f982-4b42-aefc-04fbe7e1c491" />


## Actividad 4

1. Reglas de flocking

Separación: esta regla busca evitar que los boids se amontonen. Cada objeto busca alejarse de los objetos cercanos, para calcular la dirección se revisa que objetor están dentro de un radio corto de percepción y genera vectores que apuntan en dirección opuesta a ellos.

Alineación: Busca que los boids se muevan en la misma dirección promedio a la e los objetos cercanos, logrando que el grupo tenga un movimiento colectivo coordinado, el calculo revisa los vecinos denttro del radio . promedia sus velocidades y obtiene la velocidad restando la velocidad actual de la velociad deseada

Cohesión: Mantiene unido al grupo llevando el boid hacia la posición promedio de los objetos cercanos, el calculo nos arroja el promedio de las posiciones de los oobjetos cercanos generano un centro de masa local. Luego calcula una fuerza de seek hacia ese centro atrayendolo para que esté con el grupo

2. Parametros clave

- Radio de precepción: define que objetos cercanos son considerados en las reglas
- Pesos de las reglas: multiplicadores que ajustan la influencia de separación, alineación y cohesión al combinarse en flock()
- maxSpeed: controla la velocidad maxima que puede alcanzar un boid
- maxforce: limimta la magnitud maxima de la fuerza de dirección

3. incrementé el peso de separación en el meodo flock()
```js
  sep.mult(4.0);
```
Los boids empezaron a repelerse entre ellos, dejaron de formar enjambres y adoptaron un movimiento mas caotico, los boids mantienen una buena distancia el uno del otro y rara vevz forman agrupaciones estables, por lo que la coheción quedó basicamente anulada por la magnitud de la furza de separación
<img width="656" height="252" alt="image" src="https://github.com/user-attachments/assets/f2824c22-a47c-447b-b0a6-4b58623275ea" />

# Apply
1. ![Imagen de WhatsApp 2025-10-02 a las 13 54 05_1db999e8](https://github.com/user-attachments/assets/0d7ce9cd-a133-4d7b-9178-c0442dcc2f36)

2. Codigo:
```js
let song;
let fft;
let particles = [];
let flowfield;
let cols, rows;
let scl = 20;
let inc = 0.1;
let zoff = 0;
let playing = false;

let showFlowfield = false;
let mode = 1;

let ripples = [];

function preload() {
  song = loadSound("assets/Pokemon_Atrapalos_Ya_Opening_1.mp3");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 255);
  fft = new p5.FFT();

  cols = floor(width / scl);
  rows = floor(height / scl);
  flowfield = new Array(cols * rows);

  for (let i = 0; i < 200; i++) {
    particles.push(new Particle());
  }
}

function draw() {
  background(0, 40);

  let spectrum = fft.analyze();

  let yoff = 0;
  for (let y = 0; y < rows; y++) {
    let xoff = 0;
    for (let x = 0; x < cols; x++) {
      let index = x + y * cols;
      let angle = noise(xoff, yoff, zoff) * TWO_PI * 4;
      let v = p5.Vector.fromAngle(angle);
      v.setMag(0.3);
      flowfield[index] = v;
      xoff += inc;
    }
    yoff += inc;
  }
  zoff += 0.003;

  if (showFlowfield) {
    stroke(100, 100, 255, 50);
    for (let y = 0; y < rows; y++) {
      for (let x = 0; x < cols; x++) {
        let index = x + y * cols;
        let v = flowfield[index];
        push();
        translate(x * scl, y * scl);
        rotate(v.heading());
        line(0, 0, scl / 2, 0);
        pop();
      }
    }
  }

  for (let p of particles) {
    // Ver si el particle está dentro de alguna onda
    let inRipple = false;
    for (let r of ripples) {
      let d = dist(p.pos.x, p.pos.y, r.x, r.y);
      if (d < r.size / 2) {
        inRipple = true;
        break;
      }
    }

    if (inRipple) {
      p.flock(particles); // flocking dentro de las ondas
    } else {
      p.follow(flowfield); // normal fuera de las ondas
    }

    p.update();
    p.show(spectrum);
    p.edges();
  }

  // Dibujar las ondas
  noFill();
  stroke(180, 150);
  strokeWeight(2);
  for (let i = ripples.length - 1; i >= 0; i--) {
    let r = ripples[i];
    ellipse(r.x, r.y, r.size);
    r.size += 4;
    r.alpha -= 2;
    if (r.alpha <= 0) {
      ripples.splice(i, 1);
    }
  }
}

function mousePressed() {
  if (!playing) {
    song.loop();
    playing = true;
  } else {
    if (song.isPlaying()) {
      song.pause();
    } else {
      song.play();
    }
  }

  ripples.push({ x: mouseX, y: mouseY, size: 10, alpha: 200 });
}

function keyPressed() {
  if (key === '1') {
    showFlowfield = !showFlowfield;
  } else if (key === '2') {
    mode = 1;
  } else if (key === '3') {
    mode = 2;
  } else if (key === 'C' || key === 'c') {
    // despejar partículas
    for (let p of particles) {
      p.pos = createVector(random(width), random(height));
      p.vel = createVector(0, 0);
      p.acc = createVector(0, 0);
    }
  }
}

class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxspeed = 3;
    this.prevPos = this.pos.copy();

    // Cada partícula nace roja o blanca y mantiene ese color
    this.color = random(1) < 0.5 ? color(0, 0, 255) : color(0, 255, 255);
  }

  follow(vectors) {
    let x = floor(this.pos.x / scl);
    let y = floor(this.pos.y / scl);
    let index = x + y * cols;
    let force = vectors[index];
    this.applyForce(force);
  }

  applyForce(force) {
    this.acc.add(force);
  }

  flock(particles) {
    let perception = 50;
    let steeringAlign = createVector();
    let steeringCohesion = createVector();
    let steeringSeparation = createVector();
    let total = 0;

    for (let other of particles) {
      let d = dist(this.pos.x, this.pos.y, other.pos.x, other.pos.y);
      if (other != this && d < perception) {
        steeringAlign.add(other.vel);
        steeringCohesion.add(other.pos);

        let diff = p5.Vector.sub(this.pos, other.pos);
        diff.div(d * d);
        steeringSeparation.add(diff);

        total++;
      }
    }

    if (total > 0) {
      steeringAlign.div(total);
      steeringAlign.setMag(this.maxspeed);
      steeringAlign.sub(this.vel);
      steeringAlign.limit(0.1);

      steeringCohesion.div(total);
      steeringCohesion.sub(this.pos);
      steeringCohesion.setMag(this.maxspeed);
      steeringCohesion.sub(this.vel);
      steeringCohesion.limit(0.1);

      steeringSeparation.div(total);
      steeringSeparation.setMag(this.maxspeed);
      steeringSeparation.sub(this.vel);
      steeringSeparation.limit(0.1);
    }

    this.applyForce(steeringAlign);
    this.applyForce(steeringCohesion);
    this.applyForce(steeringSeparation);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxspeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  show(spectrum) {
    let freq = spectrum[int(map(this.pos.x, 0, width, 0, spectrum.length))];
    let r = map(freq, 0, 255, 8, 25);

    noStroke();
    fill(this.color);
    if (mode === 1) {
      ellipse(this.pos.x, this.pos.y, r, r);
    } else if (mode === 2) {
      rect(this.pos.x, this.pos.y, r, r);
    }
  }

  edges() {
    if (this.pos.x > width) {
      this.pos.x = 0;
      this.updatePrev();
    }
    if (this.pos.x < 0) {
      this.pos.x = width;
      this.updatePrev();
    }
    if (this.pos.y > height) {
      this.pos.y = 0;
      this.updatePrev();
    }
    if (this.pos.y < 0) {
      this.pos.y = height;
      this.updatePrev();
    }
  }

  updatePrev() {
    this.prevPos = this.pos.copy();
  }
}

```

<img width="971" height="692" alt="image" src="https://github.com/user-attachments/assets/e1a63c2d-0f1e-41a5-bdfd-4e04f4d5012f" />

https://editor.p5js.org/nijesa/sketches/KUrIYDFqg

