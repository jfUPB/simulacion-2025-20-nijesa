# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1
#### Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.
la aleatoriedad le da una capa más artistica al arte generativo, hace que se vea muy organico la imagen en movimiento y a mi parecer, ayuda a darle vida a las ideas que tiene el autor

### Actividad 2
#### - Cuál es el papel de la aleatoriedad en su obra.
Sofia usa la aleatoriedad para hacer que el sistema parezca vivo y no repetitivo, no usa la aleatoriedad en desorden, sino en un espacio controlado, manteniendo la conexión con la musica sin que se vuelva predecible
#### - Según tu perfil profesional, cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos.
Personalmente en mis proyectos me gusta usar la aleatoriedad para los enemigos que aparecen en una zona o teniendo varios tipos de niveles o habitaciones y que se organizen de manera aleatoria, y asi cada partida sea diferente y varie si dificultad y diseño de nivel
Ejemplo: algo como lo que acabo de explicar lo alpican el varios juegos Rogue-Like, donde usan semillas para decidir la distribucion de habitaciones y asi no exista una partida igual a la otra, se aplica en habitaciones, objetos, jefes, maquinas de apuesta, etc.

### Actividad 3
Codigo original
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}

```
#### - Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
   const r = floor(random(255));
  const g = floor(random(255));
  const b = floor(random(255));
  stroke(r, g, b);
  strokeWeight(4)
  
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(7));
    if (choice == 0 || choice == 4) {
      this.x++;
    } else if (choice == 1 || choice == 6) {
      this.x--;
    } else if (choice >= 2 && choice <= 3) {
      this.y++;
    } else {
      this.y--;
    }
    
    if (this.y >= height ||this.y <= 0 ){this.y=height/2}
    if (this.x >= width ||this.x <= 0 ){this.y=width/2}
  }
}

```
#### - Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.
deberia moverse pocas veces para abajo, aumentar el tamaño del punto y cada frame pintar el punto de un color diferente, cuando llega al borde del canva vuelve al centro
#### - Ejecuta el código y escribe en tu bitácora qué sucedió realmente.
El punto se mueve mucho para abajo (-y), el tamaño del punto si cambia y es un poco mas grande que en el codigo original, cada frame cambia el color del punto
#### - Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?
Ocurrió lo que esperaba con todo, menos con el movimiento del punto, creí que poniendo dos numeros en un movimiento y en el ultimo solo un numero iba a ser un movimiento poco común

### Actividad 4
#### - En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.
En la distribución uniforme cada numero tiene la misma probabilidad de salir, mientras que una no uniforme de numero aleatorios los numeros que se acercan a la media salen mas  seguido a los que estan en los extermos, por ejemplo, entre 1y 3, que salga 2 es mas probable a que salga 1 o 3, porque esa es la media
#### - Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.
```js
step() {
    const choice = floor(randomGaussian(10));
    if (choice == 1 || choice == 2||choice == 9) {
      this.x++;
    } else if (choice == 4 || choice == 6 ) {
      this.x--;
    } else if (choice == 7 || choice == 5 ) {
      this.y++;
    } else {
      this.y--;
    }
    
    if (this.y >= height ||this.y <= 0 ){this.y=height/2}
    if (this.x >= width ||this.x <= 0 ){this.y=width/2}
  }
```

### Actividad 5
Una vez has entendido el concepto de distribución normal, vas a pensar en una nueva manera de visualizarlo.

#### - Crea un nuevo sketch en p5.js que represente una distribución normal.
#### - Copia el código en tu bitácora.
```js
let xCounts = {}; 

function setup() {
  createCanvas(400, 400);
  background(220);
  drawCartesianPlane();
  frameRate(10);
}

function draw() {
  
  let val = round(randomGaussian());
  val = constrain(val, -3, 3);

  // Contar cuántas veces ha salido ese valor
  if (xCounts[val] === undefined) {
    xCounts[val] = 0;
  }
  xCounts[val] += 1;

  // Convertir x a coordenadas de canvas
  let xCanvas = map(val, -3, 3, 0, width);

  // Usar la cantidad de apariciones para definir el valor Y en coordenadas matemáticas
  let yMath = 1 + xCounts[val] * 0.1;

  // Convertir yMath (0 a 10) a coordenadas de canvas
  let yCanvas = map(yMath, 0, 10, height, 0);

  // Dibujar el punto
  noStroke();
  fill(0, 50);
  circle(xCanvas, yCanvas, 8);
}

function drawCartesianPlane() {
  let xMin = -3;
  let xMax = 3;
  let yMin = 0;
  let yMax = 10;

  let yZeroVal = 1;
  let yZero = map(yZeroVal, yMin, yMax, height, 0);
  let xZero = map(0, xMin, xMax, 0, width);

  stroke(0);
  strokeWeight(2);

  // Eje X
  line(0, yZero, width, yZero);

  // Eje Y
  line(xZero, 0, xZero, height);

  // Ticks y etiquetas
  textAlign(CENTER, TOP);
  fill(0);
  noStroke();

  for (let x = xMin; x <= xMax; x++) {
    let px = map(x, xMin, xMax, 0, width);
    stroke(0);
    line(px, yZero - 5, px, yZero + 5);
    noStroke();
    text(x, px, yZero + 10);
  }

  textAlign(RIGHT, CENTER);
  for (let y = yMin; y <= yMax; y++) {
    let py = map(y, yMin, yMax, height, 0);
    stroke(0);
    line(xZero - 5, py, xZero + 5, py);
    noStroke();
    text(y, xZero - 10, py);
  }
}

```
La idea del codigo la hice yo, empecé programandolo solo y cuando tenia dudas se las preguntaba a chat gtp, finalmente organicé el codigo con chatgtp porque antes se veía muy raro
#### - Coloca en enlace a tu sketch en p5.js en tu bitácora.

https://editor.p5js.org/nijesa/sketches/dFp4etSzu
#### - Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
<img width="497" height="497" alt="image" src="https://github.com/user-attachments/assets/c1d6f850-eb85-49a3-93fa-675c1737a6d8" />


### Actividad 6
#### - Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.
#### - Explica por qué usaste esta técnica y qué resultados esberabas obtener.
La use porque queria tener una aleatoriedad mas controlada en el eje y, ya que no me gustó mucho como fue implementado en el codigo original, esperaba poder controlarlo más y si fue mucho más controlado, ya que cuando la probabilidad que suba o baje es mucho más controlable
#### - Copia el código en tu bitácora.
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;
let colorGen;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  colorGen = new colorin();
  
  background(255);
}

function draw() {
 const r = colorGen.rojo(walker.y / height);
const g = colorGen.verde(walker.y / height);
const b = colorGen.azul(walker.y / height);
  stroke(r, g, b);
  strokeWeight(4)
  
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    point(this.x, this.y);
  }

  step() {

    const choice = floor(randomGaussian(2));
    if (choice == 1) {
      this.x++;
    } else if (choice == 2) 
    {
      this.x--;
    } 
    
   let Number = qualifyNo();
      console.log(Number)
    if (Number>=0.54)
      {
        this.y--;
      }
      else if (Number<0.54)
      {
        this.y++;
      }
    else{}
    
    
   
    
    
    if (this.y >= height ||this.y <= 0 )
    {
     if(this.y>=height){this.y =0}
      else{this.y = height}
      
    }
    if (this.x >= width ||this.x <= 0 )
    {
      if(this.x>=width){this.x =0}
      else{this.x = width}
    }
  }
}

class colorin {
  rojo(y) {
    if (y <= 0.30) {
      return 245;
    } else if (y > 0.30 && y <= 0.44) {
      return 255;
    } else if (y > 0.44 && y <= 0.58) {
      return 255;
    } else if (y > 0.58 && y <= 0.72) {
      return 0;
    } else if (y > 0.72 && y <= 0.86) {
      return 2;
    } else {
      return 254;
    }
  }

  verde(y) {
    if (y <= 0.30) {
      return 3;
    } else if (y > 0.30 && y <= 0.44) {
      return 110;
    } else if (y > 0.44 && y <= 0.58) {
      return 235;
    } else if (y > 0.58 && y <= 0.72) {
      return 202;
    } else if (y > 0.72 && y <= 0.86) {
      return 65;
    } else {
      return 0;
    }
  }

  azul(y) {
    if (y <= 0.30) {
      return 0;
    } else if (y > 0.30 && y <= 0.44) {
      return 0;
    } else if (y > 0.44 && y <= 0.58) {
      return 1;
    } else if (y > 0.58 && y <= 0.72) {
      return 30;
    } else if (y > 0.72 && y <= 0.86) {
      return 254;
    } else {
      return 212;
    }
  }
}

function qualifyNo()
{
    let r1 = random(1);
    let prob = r1;
    let r2 = random(1);
    
    if(r2 <= prob){
      return r1;
    }
}


 //let rojo =(245, 3, 0);
  //let naranja =(255, 110, 0);
  //let amarillo =(255, 235, 1);
  //let verde =(0, 202, 30);
  //let azul =(2, 65, 254);
  //let morao =(254, 0, 212);
```
#### - Coloca en enlace a tu sketch en p5.js en tu bitácora.
https://editor.p5js.org/nijesa/sketches/I6SYqCYA1
#### - Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
<img width="797" height="295" alt="image" src="https://github.com/user-attachments/assets/715d03ee-fb02-45bc-b92d-6c5702947e3e" />



