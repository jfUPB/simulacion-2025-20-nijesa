# Unidad 2


## 🤔 Fase: Reflect

### Actividad 9
#### Parte 1: recuperación de conocimiento (Retrieval Practice)

- Escribe la “receta” del marco MOTION 101.
La receta se compone de tres vectores: position, velocity, acceleration. Para mover el objeto en p5js, se debe actializar la velocidad con la aceleración y la posición con la velocidad
```js
velocity.add(acceleration);
position.add(velocity);
```
- ¿Cómo se relaciona el marco MOTION 101 con los conceptos de position, velocidad y aceleración?
La posición cambia la posición de cada frame segun la velocidad y la velocidad cambia según la aceleración, pueden ser independientes o no dependiendo de las condiciones iniciales
- Si tuvieras que explicar el concepto de motion 101 de manera geométrica, ¿Cómo lo harías?
La posición es un punto en el plano, la velociad es una flecha que dice a donde se va a mover el punto y la aceleración modifica la flecha de la velocidad haciendo que cambie de dirección y largo
#### Parte 2: reflexión sobre tu proceso (Metacognición)

- ¿Qué fue lo más desafiante en la Actividad 08? ¿El concepto creativo, la implementación del algoritmo de aceleración o la integración de la interactividad?
Lo más desafiante fue crear el sistema de "colision" entre los objetos mover y pelota, al principio chocaban y pelota se movia extremadamente rapido, cuando arreglé eso pelota se pegaba a mover y era maluco despegarlo
- ¿Tu algoritmo de aceleración produjo el efecto que esperabas? Describe un momento “sorpresa” (esperado o inesperado) durante su desarrollo.
si, el efecto sorpresa fue cuando se hacia la colisión y la magnitud de pelota era enorme y se movia tan rapido que llegaba un momento en que desaparecia de la pantalla
- ¿Cómo ha cambiado tu forma de pensar sobre el “movimiento” en una pantalla después de esta unidad?
me recordó el uso que se le puede dar a las formulas de movimiento vistas en fisica mecanica para lograr movimientos dentro de los juegos u obras de arte generativo.
- Si tuvieras una semana más, ¿qué mejorarías o qué otro algoritmo de aceleración te gustaría experimentar?
me gustaría experimentar algo como el tiro con arco o tiro parabolico dentro de p5js
