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


