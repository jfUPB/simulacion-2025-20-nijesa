# Unidad 1

## 🛠 Fase: Apply

### Actividad 7
#### - Crea un nuevo sketch en p5.js donde los visualices.
#### - Explica el concepto qué resultados esberabas obtener.
El concepto del perlin noice me da numeros aleatorios sin hacer un salto muy grande entre ellos, lo uso en el walker para que el movimiento aleatorio sea mas organico, adicionalmente lo usé para modificar el color de los cuadrados rojos en el momento en que se dibujan
#### - Copia el código en tu bitácora.
```js
let walker;
let noiseOffset = 0.0

function setup() {
  createCanvas(400, 400);
  walker = new Walker();
  background(10,252,0); // solo una vez
  
  
  
}

function draw() {
  walker.erasePrevious();  
  walker.step();           
  walker.show(); 
  
  rosas();
  
 
}

class Walker {
  constructor() {
    this.tx = 0;
    this.ty = 10000;
    this.prevX = 0;
    this.prevY = 0;
    this.x = 0;
    this.y = 0;
  }

  step() {
    this.prevX = this.x;
    this.prevY = this.y;

    this.x = map(noise(this.tx), 0, 1, 0, width);
    this.y = map(noise(this.ty), 0, 1, 0, height);

    this.tx += 0.01;
    this.ty += 0.01;
  }


  erasePrevious() {
    // Dibuja un círculo blanco sobre la abeja anterior
    noStroke();
    fill(10,252,0);
    circle(this.prevX, this.prevY, 23); // tamaño ligeramente mayor que la abeja
  }

  show() {
    
    let r=noise(0);
    
    
    // Cuerpo
    fill(255, 200, 0);
    stroke(0);
    strokeWeight(1);
    ellipse(this.x, this.y, 10, 10);

    // Alitas
    fill(200, 230, 255, 150);
    noStroke();
    ellipse(this.x - 5, this.y - 5, 6, 10);
    ellipse(this.x + 5, this.y - 5, 6, 10);

    // Antenas
    stroke(0);
    line(this.x - 2, this.y - 6, this.x - 4, this.y - 10);
    line(this.x + 2, this.y - 6, this.x + 4, this.y - 10);
  }
  
  
}

let cont = 0;
function rosas(){
  
  if(cont<=15){
    x=random(0,width);
  y=random(90,height);
   let redValue = map(noise(noiseOffset), 0, 1, 0, 255);
    noiseOffset += 0.1; 
  noStroke();
  fill(redValue,0,0);
  rect(x,y,5);
    cont +=1;
  }
  else{return;}
  
}

function clouds(){
  
  
  
}

```
#### - Coloca en enlace a tu sketch en p5.js en tu bitácora.

https://editor.p5js.org/nijesa/sketches/KoGudwQvz

#### - Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

<img width="497" height="495" alt="image" src="https://github.com/user-attachments/assets/41c1b9f2-ffdc-4470-9e30-fb5ca1397877" />



### Act 8
#### - Un texto donde expliques el concepto de obra generativa.
La obra es de una aveja que se deja llevar por el viento para comerse las flores bajo un dia nublado
#### - Copia el código en tu bitácora.
```js
let walker;
let noiseOffset = 0.0

function setup() {
  createCanvas(400, 400);
  walker = new Walker();
  background(10,252,0); // solo una vez
  clouds();
  
  
}

function draw() {
  walker.erasePrevious();  
  walker.step();           
  walker.show(); 
  
  rosas();
  
 
}

class Walker {
  constructor() {
    this.tx = 0;
    this.ty = 10000;
    this.prevX = 0;
    this.prevY = 0;
    this.x = 0;
    this.y = 0;
  }

  step() {
    this.prevX = this.x;
    this.prevY = this.y;

    this.x = map(noise(this.tx), 0, 1, 0, width);
    this.y = map(noise(this.ty), 0, 1, 0, height);

    this.tx += 0.01;
    this.ty += 0.01;
  }


  erasePrevious() {
    // Dibuja un círculo blanco sobre la abeja anterior
    noStroke();
    fill(10,252,0);
    circle(this.prevX, this.prevY, 23); // tamaño ligeramente mayor que la abeja
  }

  show() {
    
    let r=noise(0);
    
    
    // Cuerpo
    fill(255, 200, 0);
    stroke(0);
    strokeWeight(1);
    ellipse(this.x, this.y, 10, 10);

    // Alitas
    fill(200, 230, 255, 150);
    noStroke();
    ellipse(this.x - 5, this.y - 5, 6, 10);
    ellipse(this.x + 5, this.y - 5, 6, 10);

    // Antenas
    stroke(0);
    line(this.x - 2, this.y - 6, this.x - 4, this.y - 10);
    line(this.x + 2, this.y - 6, this.x + 4, this.y - 10);
  }
  
  
}

let cont = 0;
function rosas(){
  
  if(cont<=20){
    x=random(0,width);
  y=random(90,height);
   let redValue = map(noise(noiseOffset), 0, 1, 0, 255);
    noiseOffset += 0.1; 
  noStroke();
  fill(redValue,0,0);
  rect(x,y,5);
    cont +=1;
  }
  else{return;}
  
}

function clouds(){
  
  loadPixels();
  let xoff = 0.0;

  
  for (let x = 0; x < width; x++) {
    let yoff = 0.0;

    for (let y = 0; y < 85; y++) {
      
      const bright = map(noise(xoff, yoff), 0, 1, 0, 255);
      set(x, y, color(0,0,252, floor(bright)));
      yoff += 0.01; 
    }
    xoff += 0.01; 
  }

  updatePixels();
  
}

````
#### - Coloca en enlace a tu sketch en p5.js en tu bitácora.
https://editor.p5js.org/nijesa/sketches/F5-75oL7x
#### - Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
<img width="497" height="501" alt="image" src="https://github.com/user-attachments/assets/c4479f63-bcba-4110-adac-2876452b32d4" />

