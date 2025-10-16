# Evidencias de la unidad 7

## Actividad 1

- Tu análisis de 3-4 ejemplos de Ji Lee, explicando cómo logran la conexión palabra-imagen.
- Tus propias ideas (descripción o boceto simple) para representar visualmente 2-3 palabras distintas de forma estática.

1. <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/0ab43004-7959-4f3e-b7f3-a1b91bfec410" />
En esta imagen representan el significado de la palabra clock convirtiendo la primera O por un reloj, lo que envia el mensaje y uno entiende el significado sin leer necesariamente la palabra

2. <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/b5a70d0e-71a9-4469-903b-97f8d71f0c57" />
En esta imagen la palabra Gravity está en el piso, y las letras están rotadas, mostrando el significado en la propia palabra, para esta imagen el significado se da mutuamente, si fueran letras al azar uno dice ah, se cayeron pero con Gravity uno si entiende el significado de la palabra

3. <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/f7047746-b816-4dd8-b179-0ba4ef8ae038" />
Tunnel tiene las dos n conectadas, lo que hace que se asemejen a un tunel, que entra por una n y sale por la otra

4. <img width="500" height="501" alt="image" src="https://github.com/user-attachments/assets/35a48ad4-bad1-455b-97af-8b9664184ccd" />
Esta me pareció muy interesante, ya que tuve que leerla varias veces para entender, (no se porque la estaba leyendo en desorden), pero el concepto toma caracteristicas clave de los vampiros y lo pone en la palabra, poniendo todo boca arriba y haciendo que la M se recuerde a los colmillos manchados de sangre caracteristicos de los vampiros


### Mis Ideas
1. Foco: la idea es que las O (una o ambas) sean focos que se pueden prender o apagar haciendo click, emiten luz que ilumina el fondo negro y las letras vecinas (F & C)
2. Pluma: La L se convierte en una pluma y la palabra va cayendo con un movimiento senoidal suave, que recuerda a como se cae una pluma
3. Conectar: La palabra aparece separada desde la e, luego se conecta completando la e generando un pulso electrico por toda la plabra (de izquierda a derecha ó derecha a izquierda)

## Actividad 2

### Conceptos clave
- Engine: Es el motor principal de física. Se encarga de calcular las fuerzas, colisiones y movimientos de los cuerpos en cada frame. Se actualiza continuamente en el bucle principal del programa.
- World: Representa el “mundo” físico donde existen los objetos. Todos los cuerpos, restricciones y eventos se agregan al World para ser simulados por el Engine
- Bodies: Son los objetos físicos dentro del mundo. Pueden tener distintas formas como rectángulos, círculos o polígonos. Cada cuerpo tiene propiedades como masa, fricción, rebote y posición.
- Constraint: Es una “restricción” que une dos cuerpos (como una cuerda o resorte). Permite simular comportamientos como péndulos o uniones elásticas.
- MouseConstraint: Permite interactuar con los cuerpos usando el mouse. Al hacer clic, se puede arrastrar o empujar los objetos en la simulación.


### Ejercicio 1:

Descripción: Creo un mundo donde varios objetos caen

```js
const { Engine, World, Bodies, Mouse, MouseConstraint } = Matter;

let engine, world;
let boxes = [];
let ground;
let mConstraint;
let myCanvas; 

function setup() {

  myCanvas = createCanvas(600, 400);


  engine = Engine.create();
  world = engine.world;


  for (let i = 0; i < 5; i++) {
    boxes.push(
      Bodies.circle(random(100, 500), random(0, 100), random(15, 30), {
        restitution: 0.8,
      })
    );
  }


  ground = Bodies.rectangle(300, height - 20, 600, 40, { isStatic: true });
  World.add(world, [...boxes, ground]);


  const canvasMouse = Mouse.create(myCanvas.elt); // usamos myCanvas en lugar de canvas
  const options = { mouse: canvasMouse };
  mConstraint = MouseConstraint.create(engine, options);
  World.add(world, mConstraint);
}

function draw() {
  background(30);
  Engine.update(engine);


  fill(200);
  noStroke();
  for (let b of boxes) {
    ellipse(b.position.x, b.position.y, b.circleRadius * 2);
  }


  fill(150);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, 600, 40);
}
```


