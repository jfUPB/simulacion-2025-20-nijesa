# Unidad 3

## 🔎 Fase: Set + Seek


### Fricción.
```js
let pelota;
let pista;
let arrastrando = false; 
let inicioArrastre;
let hoyoPos;

function setup() {
  createCanvas(600, 400);
  pelota = new Pelota();
  pista = new Pista();
  hoyoPos = createVector((width/2)-200,height/2);
}

function draw() {
  background(0,100,0,170);

  pista.display();
  
  fill(0)
  circle(hoyoPos.x,hoyoPos.y,20)
  
  let coefFriccion = pista.contiene(pelota.pos) ? 0.5 : 0.05;

  
  if (pelota.vel.mag() > 0) {
    let friccion = pelota.vel.copy();
    friccion.mult(-1);
    friccion.normalize();
    friccion.mult(coefFriccion);
    pelota.applyForce(friccion);
  }

  pelota.update();
  pelota.edges();
  pelota.display();
  pelota.hoyo(hoyoPos);

  
  if (arrastrando) {
    stroke(0);
    strokeWeight(2);
    line(pelota.pos.x, pelota.pos.y, mouseX, mouseY);
  }
}


class Pelota {
  constructor() {
    this.pos = createVector(width / 2, height / 2);
    this.vel = createVector(random(-5, 5), random(-5, 5));
    this.acc = createVector(0, 0);
    this.radio = 20;
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);

    
    if (this.vel.mag() < 0.1) {
      this.vel.mult(0);
    }
  }

  edges() {
    // Rebote en eje X
    if (this.pos.x < this.radio || this.pos.x > width - this.radio) {
      this.vel.x *= -0.7; 
      this.pos.x = constrain(this.pos.x, this.radio, width - this.radio);
    }
    // Rebote en eje Y
    if (this.pos.y < this.radio || this.pos.y > height - this.radio) {
      this.vel.y *= -0.7;
      this.pos.y = constrain(this.pos.y, this.radio, height - this.radio);
    }
  }

  display() {
    let c1='200, 100, 100'
    fill(c1);
    stroke(0);
    circle(this.pos.x, this.pos.y, this.radio * 2);
  }

  dentroDePelota(x, y) {
    return dist(x, y, this.pos.x, this.pos.y) < this.radio;
  }
  hoyo(posHoyo){
    if(this.pos.x<=posHoyo.x+5 && this.pos.x>=posHoyo.x - 5 && this.pos.y <= posHoyo.y +5 && this.pos.y >= posHoyo.y - 5)
    {
       print("Entró en el hoyo");      
    }
  }
}


class Pista {
  constructor() {
    
    this.x = width / 4;
    this.y = height / 4;
    this.w = width / 2;
    this.h = height / 2;
  }

  display() {
    noStroke();
    fill(180, 220, 180, 150); // verde transparente
    rect(this.x, this.y, this.w, this.h);
  }

  contiene(pos) {
    return pos.x > this.x && pos.x < this.x + this.w &&
           pos.y > this.y && pos.y < this.y + this.h;
  }
}


function mousePressed() {
  if (pelota.dentroDePelota(mouseX, mouseY)) {
    arrastrando = true;
    inicioArrastre = createVector(mouseX, mouseY);
  }
}

function mouseReleased() {
  if (arrastrando) {
    arrastrando = false;
    // vector desde mouse hasta la pelota (como un "slingshot")
    let direccion = createVector(pelota.pos.x - mouseX, pelota.pos.y - mouseY);
    direccion.mult(0.1); // escala de fuerza
    pelota.applyForce(direccion);
  }
}

```
La fricción fue modelada a partir de su coeficiente, este cambia dependiendo de que superficie tenga debajo la pelota, la fuerza de fricción se modela a partir de la velocidad de la pelota, copiandola, multiplicando la copia por -1 y por el coeficiente luego de haber normalizado la copia y ya despues se aplica, esto hace que el coeficiente sea negativo (reste velocidad) y dependa del coeficiente para saber que tanta fuerza se resta al objeto
La idea es que estamos en un campo de golf, donde la idea es meterla pelota en el hueco usando la fricción del terreno para saber que tan duro se le tiene que pegar a la pelota
Link (https://editor.p5js.org/nijesa/sketches/kfkyJPcSQ)
<img width="727" height="493" alt="image" src="https://github.com/user-attachments/assets/47d87a9f-1ce8-4098-a8d4-ec301cf50067" />


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
<img width="571" height="512" alt="image" src="https://github.com/user-attachments/assets/c240744f-d2f7-46ec-9861-95b96ed4b1d5" />

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


### Copia el enlace a tu ejemplo en p5.js.
### Copia el código.
### Captura una imagen representativa de tu ejemplo.


