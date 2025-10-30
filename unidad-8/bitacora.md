# Evidencias de la unidad 8

## Actividad 1
Noté una conexión muy organica entre el sonido y la imagen, los visuales reaccionan de manera sutil al piano y el ambiente, generando paisajes que se expanden y contraen al ritmo del sonido. tambien pude notar como en otra los visuales reaccionan  de manera mas ritmica y fragmentada con los beats y las voces, estos tienen un movimiento más marcado.
Cada elemento generativo se evidencia en como las texturass y patrones van evolucionando con el tiempo, no como animaciones pregrabadas sino como sistemas visuales que se transforman de manera constante; lineas que se multiplican, manchas de color que se disuelven, geometrías que emergen y desaparecen. Estos comportamientos sugieren que cada performance es única, incluso si se repitiera con la misma música, porque las visualizaciones parecen depender de parámetros aleatorios y de la interpretación en vivo de la artista.
En cuanto a la sensación de “liveness”, se percibe una interacción real entre sonido, imagen y artista. Viendo como los visuales se generan en tiempo real hace que la experiencia se sienta viva, impredecible y emocionalmente conectada con la musica. El resultado no es una proyección estatica, sino una conversación entre el código, el sonido y el instante presente


## Actividad 2

Pieza musical elegida:
El amar y el querer — José José[🎧 Escuchar en YouTube](https://www.youtube.com/watch?v=RBicJYn3tGw&list=RDRBicJYn3tGw&start_radio=1)

Descripción del concepto visual:
La visualización está inspirada en la dualidad emocional que presenta la canción: la calma del amor y la intensidad del querer. Durante las partes suaves y melancólicas, el escenario muestra un paisaje sereno con tonalidades verdes y azul claro, transmitiendo paz y quietud. Cuando llega el coro —más pasional y dramático—, el entorno se transforma en una tormenta digital: los colores se tornan fríos y eléctricos (azules, morados y blancos), y las formas se agitan violentamente, evocando el caos emocional del querer frente a la calma del amor.

Inputs seleccionados y justificación:
- Tecla T: activa el modo tormenta. Este estado refleja la intensidad emocional de la canción durante el coro.
  - Presionar un número del 1 al 9 controla la fuerza de la tormenta:
    - 1 representa una tormenta suave y difusa.
    - 9 simboliza una tormenta caótica, con gran movimiento, viento y rayos.

- Tecla C: activa el modo calma, correspondiente a los momentos más tranquilos de la canción.
   - Presionar un número del 1 al 9 ajusta el grado de movimiento dentro de la calma:
      - 1 genera un paisaje casi estático.
      - 9 incluye leves movimientos de hojas o partículas flotando lentamente.

- Tecla M: hace que caigan migajas o partículas desde la parte superior, aportando un elemento visual poético que puede representar recuerdos o emociones que se desvanecen.

Algoritmos o técnicas planeadas:

- Sistemas de partículas: para crear tanto las migajas como la lluvia y los rayos de la tormenta.

- Ruido Perlin (Perlin noise): para los movimientos suaves de las hojas o elementos del paisaje en el modo calma.

- Física básica (p5.Vector y gravedad simulada): para dar peso y naturalidad al movimiento de las partículas.

- Interpolaciones de color: para hacer transiciones suaves entre los estados de calma y tormenta, generando un contraste emocional y visual claro.

Bocetos y explicación de la interacción:
En los bocetos, la escena inicial muestra un paisaje con tonos verdes y movimientos lentos, representando la calma. Al presionar T, el color de fondo cambia progresivamente hacia azules y morados, mientras que las partículas aumentan en cantidad y velocidad, simulando lluvia y viento.
Con C, todo vuelve a un estado sereno; las partículas disminuyen y se desplazan con movimientos ondulantes, casi imperceptibles. Al presionar M, pequeñas formas blancas descienden lentamente, añadiendo un toque nostálgico.
