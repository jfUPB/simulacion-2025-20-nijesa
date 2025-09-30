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


## Actividad 3

1. Estructura de datos usada para el campo de flujo y como se generan sus vectores

Separación: esta regla busca evitar que los boids se amontonen. Cada objeto busca alejarse de los objetos cercanos, para calcular la dirección se revisa que objetor están dentro de un radio corto de percepción y genera vectores que apuntan en dirección opuesta a ellos.

Alineación: Busca que los boids se muevan en la misma dirección promedio a la e los objetos cercanos, logrando que el grupo tenga un movimiento colectivo coordinado, el calculo revisa los vecinos denttro del radio . promedia sus velocidades y obtiene la velocidad restando la velocidad actual de la velociad deseada

Cohesión: Mantiene unido al grupo llevando el boid hacia la posición promedio de los objetos cercanos, el calculo nos arroja el promedio de las posiciones de los oobjetos cercanos generano un centro de masa local. Luego calcula una fuerza de seek hacia ese centro atrayendolo para que esté con el grupo


2. Parametros clave

- Radio de precepción: define que objetos cercanos son considerados en las reglas
- Pesos de las reglas: multiplicadores que ajustan la influencia de separación, alineación y cohesión al combinarse en flock()
- maxSpeed: controla la velocidad maxima que puede alcanzar un boid
- maxforce: limimta la magnitud maxima de la fuerza de dirección

3. a
4. a

