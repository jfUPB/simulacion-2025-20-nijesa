# Evidencias de la unidad 8

## Actividad 1
Noté una conexión muy organica entre el sonido y la imagen, los visuales reaccionan de manera sutil al piano y el ambiente, generando paisajes que se expanden y contraen al ritmo del sonido. tambien pude notar como en otra los visuales reaccionan  de manera mas ritmica y fragmentada con los beats y las voces, estos tienen un movimiento más marcado.
Cada elemento generativo se evidencia en como las texturass y patrones van evolucionando con el tiempo, no como animaciones pregrabadas sino como sistemas visuales que se transforman de manera constante; lineas que se multiplican, manchas de color que se disuelven, geometrías que emergen y desaparecen. Estos comportamientos sugieren que cada performance es única, incluso si se repitiera con la misma música, porque las visualizaciones parecen depender de parámetros aleatorios y de la interpretación en vivo de la artista.
En cuanto a la sensación de “liveness”, se percibe una interacción real entre sonido, imagen y artista. Viendo como los visuales se generan en tiempo real hace que la experiencia se sienta viva, impredecible y emocionalmente conectada con la musica. El resultado no es una proyección estatica, sino una conversación entre el código, el sonido y el instante presente


## Actividad 2

Pieza musical elegida:
El amar y el querer — José José[🎧 Escuchar en YouTube](https://www.youtube.com/watch?v=RBicJYn3tGw&list=RDRBicJYn3tGw&start_radio=1)

Descripción del concepto visual:
La visualización está inspirada en la dualidad emocional que presenta la canción: la calma del amor y la intensidad del querer. Durante las partes suaves y melancólicas, el escenario muestra un paisaje sereno con tonalidades verdes y azul claro, transmitiendo paz y quietud. Cuando llega el coro —más pasional y dramático—, el entorno se transforma en una tormenta digital: los colores se tornan fríos y eléctricos (azules, morados y blancos), y las formas se agitan violentamente, evocando el caos emocional del querer frente a la calma del amor.

Inputs seleccionados y justificación:
- Tecla T: activa el modo tormenta. Este estado refleja la intensidad emocional de la canción durante el coro.
  - Presionar un número del 1 al 9 controla la fuerza de la tormenta:
    - 1 representa una tormenta suave y difusa.
    - 9 simboliza una tormenta caótica, con gran movimiento, viento y rayos.

- Tecla C: activa el modo calma, correspondiente a los momentos más tranquilos de la canción.
   - Presionar un número del 1 al 9 ajusta el grado de movimiento dentro de la calma:
      - 1 genera un paisaje casi estático.
      - 9 incluye leves movimientos de hojas o partículas flotando lentamente.

- Tecla M: hace que caigan migajas o partículas desde la parte superior, aportando un elemento visual poético que puede representar recuerdos o emociones que se desvanecen.

Algoritmos o técnicas planeadas:

- Sistemas de partículas: para crear tanto las migajas como la lluvia y los rayos de la tormenta.

- Ruido Perlin (Perlin noise): para los movimientos suaves de las hojas o elementos del paisaje en el modo calma.

- Física básica (p5.Vector y gravedad simulada): para dar peso y naturalidad al movimiento de las partículas.

- Interpolaciones de color: para hacer transiciones suaves entre los estados de calma y tormenta, generando un contraste emocional y visual claro.

Bocetos y explicación de la interacción:
En los bocetos, la escena inicial muestra un paisaje con tonos verdes y movimientos lentos, representando la calma. Al presionar T, el color de fondo cambia progresivamente hacia azules y morados, mientras que las partículas aumentan en cantidad y velocidad, simulando lluvia y viento.
Con C, todo vuelve a un estado sereno; las partículas disminuyen y se desplazan con movimientos ondulantes, casi imperceptibles. Al presionar M, pequeñas formas blancas descienden lentamente, añadiendo un toque nostálgico.



## Actividad 3

Codigo:
```js
let song, fft, amp;
let particles = [];
let mode = "calma"; 
let intensidad = 1;
let colorFondo;

function preload() {
  song = loadSound('assets/el_amar_y_el_querer.mp3');
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100);
  fft = new p5.FFT(0.8, 128);
  amp = new p5.Amplitude();
  colorFondo = color(120, 40, 40);
  noStroke();
}

function draw() {
  background(colorFondo);

  // Analiza el sonido
  let spectrum = fft.analyze();
  let waveform = fft.waveform();
  let bass = fft.getEnergy("bass");
  let treble = fft.getEnergy("treble");
  let level = amp.getLevel();

  // --- BARRAS REACTIVAS ---
  let barWidth = width / spectrum.length;
  for (let i = 0; i < spectrum.length; i++) {
    let x = i * barWidth;
    let h = map(spectrum[i], 0, 255, 0, height / 3);
    fill(0, 0, 100);
    rect(x, height / 2 - h / 2, barWidth - 2, h, 3);
  }

  // --- ONDA SUPERIOR ---
  noFill();
  stroke(200, 80, 100);
  strokeWeight(3);
  beginShape();
  for (let i = 0; i < waveform.length; i++) {
    let x = map(i, 0, waveform.length, 0, width);
    let y = height / 2 - map(waveform[i], -1, 1, 0, height / 6);
    curveVertex(x, y + sin(i * 0.05 + frameCount * 0.01) * 5);
  }
  endShape();

  // --- ONDA INFERIOR ---
  stroke(40, 80, 100);
  strokeWeight(3);
  beginShape();
  for (let i = 0; i < waveform.length; i++) {
    let x = map(i, 0, waveform.length, 0, width);
    let y = height / 2 + map(waveform[i], -1, 1, 0, height / 6);
    curveVertex(x, y + sin(i * 0.05 - frameCount * 0.01) * 5);
  }
  endShape();

  noStroke();

  // --- PARTÍCULAS ---
  for (let p of particles) {
    p.update(mode, intensidad, bass, treble, level);
    p.display();
  }

  // Generar menos partículas
  if (frameCount % 8 === 0) {
    if (mode === "calma") {
      particles.push(new Particle(random(width), random(height), color(120 + random(-10,10), 40, 100)));
      colorFondo = lerpColor(colorFondo, color(120, 40, 40), 0.02);
    } 
    else if (mode === "tormenta") {
      particles.push(new Particle(random(width), random(height), color(220 + random(-20,20), 80, 100)));
      colorFondo = lerpColor(colorFondo, color(240, 40, 60), 0.05);
    } 
    else if (mode === "migajas") {
      particles.push(new Particle(random(width), random(height), color(40, 80, 100)));
      colorFondo = lerpColor(colorFondo, color(50, 30, 40), 0.03);
    }
  }

  // Limitar cantidad máxima
  if (particles.length > 200) particles.splice(0, 10);
}

// Clase de partículas con movimiento orgánico más lento
class Particle {
  constructor(x, y, c) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.size = random(3, 8);
    this.color = c;
    this.noiseOffset = createVector(random(1000), random(1000));
  }

  update(mode, intensidad, bass, treble, level) {
    let noiseScale = 0.002 + 0.0005 * intensidad;
    let angle = noise(this.noiseOffset.x, this.noiseOffset.y) * TWO_PI * 2;
    let dir = p5.Vector.fromAngle(angle);

    // Movimiento más lento y orgánico
    this.acc = dir.mult(0.2 + level * 2);

    if (mode === "calma") {
      this.acc.mult(0.2 + intensidad * 0.05);
      this.color = color(120 + random(-5, 5), 40, 100);
    } 
    else if (mode === "tormenta") {
      this.acc.rotate(random(-PI / 6, PI / 6));
      this.acc.mult(0.8 + intensidad * 0.2);
      this.color = color(220 + random(-20, 20), 80, 100);
    } 
    else if (mode === "migajas") {
      this.acc.mult(0.1 + intensidad * 0.03);
      this.color = color(40, 80, 100);
    }

    this.vel.add(this.acc);
    this.vel.limit(1.5 + intensidad * 0.3);
    this.pos.add(this.vel);

    // Envolvente (mantiene en pantalla)
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.y < 0) this.pos.y = height;
    if (this.pos.y > height) this.pos.y = 0;

    this.noiseOffset.add(0.005, 0.005);
  }

  display() {
    noStroke();
    fill(this.color);
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}

function keyPressed() {
  if (key === 'C' || key === 'c') mode = "calma";
  if (key === 'T' || key === 't') mode = "tormenta";
  if (key === 'M' || key === 'm') mode = "migajas";
  if (key >= '1' && key <= '9') intensidad = int(key);
}

function mousePressed() {
  if (!song.isPlaying()) {
    song.loop();
  } else {
    song.pause();
  }
}


```

Captura:

![Uploading image.png…]()


[link](https://editor.p5js.org/nijesa/sketches/DvAlTgiwIs)
