# Unidad 3

## 🔎 Fase: Set + Seek


### Fricción.


### Resistencia del aire y de fluidos.

``` js
let hojas = []; 

function setup() {
  createCanvas(400, 400);

  
  for (let i = 0; i < 50; i++) { 
    hojas.push(new hoja());
  }
}

function draw() {
  background(0,200,255);
  tronco();

  let viento = vientoVector();
  let gravedad =fuerzaGravedad();
  // Aplicamos viento y dibujamos todas las hojas
  for (let h of hojas) {
    h.applyWind(gravedad);
    h.applyWind(viento);
    h.diseno();
  }
}

function tronco(){
  let ancho=40;
  let largo=250;
  noStroke();
  fill('#905010');
  rect((width/2)-(ancho/2),height-largo,ancho,largo);
}


function vientoVector() {
  let center = createVector(width/2, height/2);
  let mouse = createVector(mouseX, mouseY);


  let dir = p5.Vector.sub(center, mouse);
  let d = dir.mag();

  if (d === 0) return createVector(0, 0);

 
  dir.normalize();

 
  let maxDiagHalf = sqrt(width*width + height*height) / 2;
  let strength = map(d, 0, maxDiagHalf, 0, random(0.01,0.3), true);
  print(strength);

  dir.mult(strength);
  return dir;
}

function fuerzaGravedad(){
  
  return createVector(0,0.098)
  
}

class hoja {
  constructor() {
   
    let largoTronco = 250;
    this.posBase = createVector(
      random(((width/2)-25), ((width/2)+25)), 
      random((height - largoTronco) - 10, (height - largoTronco) + 10)
    ); 

    
    this.posPunta = createVector(random(-100, 100), random(75, 150));

    
    this.mass = 5;
    this.velocity = createVector(0, 0);
    this.accel = createVector(0, 0);

   
    this.longitud = this.posPunta.mag();
  }
  

  applyWind(force) {
   
    this.accel.add(force);
    this.velocity.add(this.accel);
    this.posPunta.add(this.velocity);
  
    this.accel.mult(0);

   
    this.posPunta.setMag(this.longitud);
  }

  diseno() {
    push();
    translate(this.posBase.x, this.posBase.y);

    stroke('#1e8c3a');   // borde verde oscuro
    strokeWeight(10);
    line(0, 0, this.posPunta.x, this.posPunta.y);

    pop();
  }
}

```
la fuerza del viento (fluido) la modelé con un vector que tiene que ver con el mouse, la dirección es desde la posición del mouse hasta el centro del canvas y la magnitud depende de que tan cerca o que tan lejos está el mouse del centro, a mayor distancia
La obra generativa es una palmera cuyas hojas se mueven con el viento
Link (https://editor.p5js.org/nijesa/full/RzBqAJK1H)

### Atracción gravitacional.
```js
let puntos = [];
let centro;
let direccion = 1; 
let maxPuntos = 15; 

function setup() {
  createCanvas(600, 600);
   c1 = color(255, 0, 0);     
  c2 = color(255, 200, 0);
  centro = createVector(width/2, height/2);
}

function draw() {
  background(0,0,0,50);
  amt = map(mouseX, 0, width, 0, 1);
  let c = lerpColor(c1, c2, amt);
  let tamano = lerp(20,30,amt);
  
  fill(c);
  noStroke();
  ellipse(centro.x, centro.y, tamano);

  
  for (let p of puntos) {
    
    let fuerzaAtrac = p5.Vector.sub(centro, p.posPunta);
    let fuerzaInt = fuerzaAtrac.copy(); 
    fuerzaInt.normalize();

   
    fuerzaAtrac = createVector(-fuerzaAtrac.y, fuerzaAtrac.x); 
    fuerzaAtrac.setMag(-0.06 * direccion); 
    
    p.applyWind(fuerzaAtrac);  
    p.applyWind(fuerzaInt);    
    p.update();
    p.show();
  }
}

function mousePressed() {
  if (puntos.length < maxPuntos) {
    
    puntos.push(crearPuntoAleatorio());
  } else {
    
    puntos.shift(); 
    puntos.push(crearPuntoAleatorio()); 
  }
  direccion *= -1; 
}

function crearPuntoAleatorio() {
  let ang = random(TWO_PI);
  let r = random(100, 200);
  let x = centro.x + cos(ang) * r;
  let y = centro.y + sin(ang) * r;
  return new Punto(x, y);
}


class Punto {
  constructor(x, y) {
    this.posPunta = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(2);
    this.accel = createVector(0, 0);
  }

  applyWind(force) {
    this.accel.add(force);
    this.velocity.add(this.accel);
    this.posPunta.add(this.velocity);
    this.accel.mult(0);
  }

  update() {
    
    this.velocity.mult(0.98);
  }

  show() {
    let r = random(0,252);
    let g = random(0,252);
    let b = random(0,252);
    let amt = map(mouseX, 0, width, 0, 1);
    let size = lerp(5,15,amt);
    fill(r, g, b);
    noStroke();
    ellipse(this.posPunta.x, this.posPunta.y, size);
  }
}

```

La fuerza la modelé creando un vector hacia el centro, este lo duplique y converti uno en velocidad tangencial, esto porque al principio lo tenia solo como tangencial y los puntos rotaban pero se alejaban, normalicé el vector que da al centro
Se relaciona con la fuerza ya que los puntos orbitan a la bola que hay en el centro asemejandose al movimiento planetario y aveces atomico (electrones con el nucleo)
Link (https://editor.p5js.org/nijesa/sketches/2b5jLGbuc)
<img width="437" height="483" alt="image" src="https://github.com/user-attachments/assets/f6354f73-3cab-4743-8dba-a2579c0a6dcb" />


### Explica cómo modelaste cada fuerza.
### Conceptualmente cómo se relaciona la fuerza con la obra generativa.
### Copia el enlace a tu ejemplo en p5.js.
### Copia el código.
### Captura una imagen representativa de tu ejemplo.

