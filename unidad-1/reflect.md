# Unidad 1

## 🤔 Fase: Reflect

### Act 9

#### Parte 1: recuperación de conocimiento (Retrieval Practice)

##### - Describe la diferencia fundamental entre la aleatoriedad generada por random() y la apariencia de aleatoriedad del Ruido Perlin (noise()). ¿En qué tipo de situación usarías cada una?
random genera numeros aleatorios entre dos valores, al ser completamente aleatorio puede haber saltos grandes o saltos pequeños mientras que el ruido perlin genera numeros aleatorios entre 0 y 1 haciendo saltos pequeños, osea que los numeros que devuelve son aleatorios cercanos al valor anterior, por ejemplo: 0.36->0.38->0.37
##### - Explica con tus palabras qué es una distribución de probabilidad. ¿Qué diferencia visual produce una caminata aleatoria con una distribución uniforme versus una con una distribución normal?
la distribuicion de probabilidad nos genera numeros aleatorios bajo unas condiciones especifica, esto nos permite modificar probabilidades para que algo tenga más probabilidad que lo otro, en la caminata aleatoria la distribucion normal hace que el objeto se mueva mas seguido para un lado que a otro, mientras que la uniforme hace que se mueva aleatoriammente con la misma probabilidad a cualquier lado
##### - ¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple
la aleatoriedad hace parecer mas organico al arte generativo, por ejemplo, usando en noise podemos hacer patrones que recuerdan a las nubes o a un cielo nublado, con el random se puede hacer que se generen varios objetos de colores aleatorios o que un solo objeto cambie su color en tiempo real
##### - Piensa en tu obra final (Actividad 08). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas.
uso el perlin noise en mi obra final en tres partes, el patron de las nubes de la parte superior, la caminata aleatoria que realiza la abeja y la intensidad en el color de las flores, es adecuado ya que me permite darle un toque un poco más realista y organico a la obra, la abeja se mueve de manera aleatoria sin dar pasos muy grandes todo el tiempo lo que se llega a parecer al movimiento que hacen los insectos en la vida real y en las flores la intensidad se puede ver como la edad que tiene cada flor
##### - ¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?
una caminata en el contexto de simulación es cuando un objeto se mueve en tiempo real con durecciones aleatorias cada vezz que se va a mover, con el Lévy flight podemos modificar la probabilidad de la dirección que toma el objeto en el movimiento, haciendo que sea más probable que vaya en una dirección de nuestra elección
#### Parte 2: reflexión sobre tu proceso (Metacognición)

##### - ¿Cuál fue el concepto más abstracto o difícil de “visualizar” para ti en esta unidad? ¿Qué hiciste para finalmente comprenderlo?
el más dificil fue el Lévy flight, tuve que leer varias veces esa parte en la pagina, preguntarle a chatgtp y ver ejemplos para poder entenderlo
##### - Describe un momento durante el desarrollo de tu obra final (Actividad 08) en el que un “error” o un resultado inesperado te llevó a una idea creativa interesante.
al principio tenia pensado en que las flores fueran de un solo color y se movieran un poco, pero finalmente me causo muchos problemas y vi que podia usar el noise() para la intensidad de su color
##### - Al combinar diferentes técnicas de aleatoriedad, ¿Cuál fue el mayor desafío? ¿La interacción entre los sistemas, el control de los parámetros, el rendimiento?
el control de parametros me parecio mas dificil, ya que al no tener mucha experiencia con js, no entiendo bien como se hacen arreglos, aveces programando uso metodos o pongo la firma completa de una variable/constante, pero me he ido acostumbrando a programar en este lenguaje de a poquitos
##### -  Si tuvieras que empezar la Actividad 08 de nuevo, ¿Qué harías de manera diferente basándote en lo que sabes ahora?
me gustaria poner otra abeja y hacer que se mevan las flores un poco y vuelvan a su posicion original
