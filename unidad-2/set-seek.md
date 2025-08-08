# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1
#### - ¿Cómo funciona la suma dos vectores en p5.js?
para sumar vetores en p5js se usa el objeto p5.vector y para sumarlos se usa .add(), por ejemplo: position.add(velocity); , en esta linea le estas sumando velocity a position, osea position = position + velocity Tambien es posible crear un nuevo vector sin modificar los vectores originales

let sumaV =  p5.Vector.add(position, velocity);
#### - ¿Por qué esta línea position = position + velocity; no funciona?
esta linea no funciona porque ambos posiiton y velocity son vectores y no numeros, en p5js no se puede sumar objetos de esa manera, lo más parecido seria concatenar ambos objetos como se hace con strings, y aun asi esa operación no haria nada.

### Actividad 2
#### - ¿Qué tuviste que hacer para hacer la conversión propuesta?
crear un vector posición donde las condiciones iniciales son width/2, height/2, cambie todos los this.x y this.y por this.position.x o .y depende de lo que necesite

### - Muestra el código que utilizaste para resolver el ejercicio.
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  // Creating the Walker object!
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(width / 2, height / 2)
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    const r = random(1);
    // A 40% of moving to the right!
    if (r < 0.4) {
      this.position.x++;
    } else if (r < 0.6) {
      this.position.x--;
    } else if (r < 0.8) {
      this.position.y++;
    } else {
      this.position.y--;
    }
    this.x = constrain(this.position.x, 0, width - 1);
    this.y = constrain(this.position.y, 0, height - 1);
  }
}
```
### Actividad 3

#### Experimenta
Dale una mirada a este código:
```js
let position;

function setup() {
    createCanvas(400, 400);
    position = createVector(6,9);
    console.log(position.toString());
    playingVector(position);
    console.log(position.toString());
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}
```
📤 Bitácora

#### ¿Qué resultado esperas obtener en el programa anterior?
Que se impriman en consola los valores del vector position dados en el setup() y luego los que se le asignan en playingVector()
#### ¿Qué resultado obtuviste?
Imprimió en la consola:

p5.Vector Object : [6, 9, 0] 
p5.Vector Object : [20, 30, 0] 
Only once 

#### Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.
Paso por valor
```js
function cambiar(x) {
  x = 10;
}

let num = 5;
cambiar(num);
console.log(num); //  5 (no cambia)

```

Paso por referencia
```js
function modificar(obj) {
  obj.valor = 100;
}

let miObjeto = { valor: 20 };
modificar(miObjeto);
console.log(miObjeto.valor); // 👉 100 (sí cambia)

```
#### ¿Qué tipo de paso se está realizando en el código?
Se está realizando paso por referencia, ya que p5.vector es un objeto
#### ¿Qué aprendiste?
- Los vectores en p5.js son objetos (p5.Vector), y como tal, se pasan por referencia.
- Esto significa que al modificar un vector dentro de una función, se afecta directamente el valor original fuera de esa función.
- Esta característica es muy útil en simulaciones de movimiento, donde puedes actualizar la posición, velocidad, o aceleración de un objeto modificando sus vectores directamente.

### Actividad 4

Explora posibilidades

#### ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
El metodo mags() retorna la magnitud del vector, y magSq() retorna el cuadrado de la magnitud del vector, no la raiz cuadrada
la más eficiente es la magSq() porque evita calcular la raiz cuadrada
#### ¿Para qué sirve el método normalize()?
normalize() convierte un vector en uno de logitud 1 apuntano en la misma dirección, sirve para mantener la direccción de in vector pero eliminar su magnitud, sirve cuando solo se necesita saber la dirección

#### Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
le diria que sirve para saber el parecido entre la dirección de dos vectores

#### El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?
Instancia: se llama desde un objeto p5.vector, usando otro como argumento
Estatico: se llama directamente desde la clase p5.vector

#### Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
La dirección resultante es perpendicular a ambos vectores, osea, si tiene un angulo de 0° y uno de 90°, el vector resultante seria de 45°. EN cuanto a la magnitud, esta seria el area de un paralelogramo formado por los dos vectores

#### ¿Para que te puede servir el método dist()?

dist() calcula la distancia entre dos vectores, sirve para proximidad, colisiones o seguir a un objetivo

#### ¿Para qué sirven los métodos normalize() y limit()?
normalize sirve para poner la magnitud de un vector en 1
limit: le da limite a la magnitud de un vector, limit(5) la magnitud del vector no puede pasar de 5


### Actividad 5
```js
let dV1 = true;
let amt = 0;
let c1, c2;
function setup() {
    createCanvas(500, 500);
   c1 = color(255,0,0);
   c2 = color(0,0,255);
}


function draw() {
  background(200);

  let amtC = map(amt, 0, 1, 0, 1);
  let c = lerpColor(c1, c2, amtC);

  let v0 = createVector(50, 50);
  let v1 = createVector(250, 0);
  let v2 = createVector(0, 250);
  let v3 = p5.Vector.lerp(v1, v2, amt);
  let v4 = p5.Vector.sub(v2, v1);

  drawArrow(v0, v1, c1);       
  drawArrow(v0, v2, c2);      
  drawArrow(v0, v3, c);       
  drawArrow(p5.Vector.add(v0, v1), v4, 'green');  
  
  if (dV1 == true) {
    amt += 0.01;
  } else {
    amt -= 0.01;
  }

  if (amt >= 1) {
    dV1 = false;
  } else if (amt <= 0) {
    dV1 = true;
  }
}


function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
#### - ¿Cómo funciona lerp() y lerpColor().

lerp: realiza una interpolación lineal, osea que devuelve un valor entre start y stop según el porcentaje el amt (amt va de 0-1), el amt sería el porcentaje del valor entre start y stop, por ejemplo, si el start es 0 y stop es 10, con un amt de 0.5, el valor retornado sería 5

lerpColor: es parecido a lerp, solo que los valores de start y stop no son numeros sino colores, dependen del amt
#### - ¿Cómo se dibuja una flecha usando drawArrow()? 

esta funcion dibuja una flecha desde un punto base con la longitud del vector, tambien pide un color.

### Actividad 6
####  - Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.

es un modelo que sirve para simular el movimiento en el que un objeto con una posición, velocidad y opcionalmente aceleración, tiene estas caracteristicas representadas como vectores. Geometricamente la posición nos dice donde está el objeto, la velocidad la dirección y la rapidez, y la aceleración en cada fotograma.

####  - ¿Cómo se aplica motion 101 en el ejemplo?

```js
let mover;

function setup() {
  createCanvas(600, 400);
  mover = new Mover();
}

function draw() {
  background(220);

  mover.update();
  mover.dibujar();
  mover.checkEdges();
}

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0.2, -0.1+0.98); 
  }

  update() {
    this.velocity.add(this.acceleration); 
    this.position.add(this.velocity);     
  }

  dibujar() {
    fill(50, 100, 200);
    stroke(0);
    ellipse(this.position.x, this.position.y, 32, 32);
  }

  checkEdges() {
    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1;
    }
    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1;
    }
  }
}

```


### Act 7
Aceleración constante

con la aceleración constante, el objeto comienza en reposo o con una VI, esta crece uniformemente con el tiempo, osea que si posición cambia cada vez más rapido
Aceleración constante en la clase de la actividad anterior
```js
constructor() {
    this.position = createVector(0, 0);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0.001, 0.001);
```

Aceleración Aleatoria
Con la aceleración aleatoria la trayectoria es imposible de conocer, los valores iniciales afectan poco en la trayectoria
Ejemplo con Aceleración aleatoria
```js
constructor() {
    this.position = createVector(width/2, height/2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(random(-5,5), random(-5,5)); 
  }

  update() {
    this.velocity.add(p5.Vector.random2D().mult(2)); 
    this.position.add(this.velocity);     
  }
```
Aceleración hacia el mouse
El objeto persigue al mouse organicamente
Ejemplo con Aceleración hacia el mouse
```js
 update() {
     // Crear un vector hacia el mouse
    let mouse = createVector(mouseX, mouseY);
    this.acceleration = p5.Vector.sub(mouse, this.position); 
    this.acceleration.setMag(0.2); 

    this.velocity.add(this.acceleration); 
    this.position.add(this.velocity);      
  }
```
