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


