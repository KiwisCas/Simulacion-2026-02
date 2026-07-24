# Unidad 1

---
## Actividad 1, La aleatoriedad en el arte generativo
---

### Distribución Uniforme
<img width="901" height="223" alt="image" src="https://github.com/user-attachments/assets/99aa8edb-e9f5-44e9-a273-1f67c3e6dd30" />

En el modelo de distribución uniforme se puede entener como dentro de un rango establecido, un modelo de probabilidad hace que un rango de valores tienda a ocurrir casi todos en su mayoría, dando una sensación de "uniformidad" a lo largo de la selección de los valores ya que no hay una tendencia explícita en su totalidad, por ejemplo se puede observar como, a lo largo de una recta, se muestra una serie de valores (en negro) los cuales se desplazan a lo largo de la recta de una forma casi equivalente en todos sus valores

---

### Distribución Gaussiana
<img width="901" height="223" alt="image" src="https://github.com/user-attachments/assets/6b3c357e-083f-48ae-a594-c153d215870f" />

En cambio, los modelos de probabilidad gaussianos dentro de un rango establecido y, a comparación de los modelos uniformes, este tiene valores de mucho más definidos ya que muchas veces hay ciertos valores que no tienden a ocurrir dentro del rango de datos debido a la media y la desviación estandar (principalmente) que se encarga de que existan ciertos datos que no se alejen de la media, haciendo que la tendencia sea mucho más grande o más pequeña como se aprecia en la imagen

---
## Actividad 2, Caminatas aleatorias
---
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

La ejecución de este código da paso a algo como lo que se va a apreciar en la siguiente imagen 
---
<img width="613" height="237" alt="image" src="https://github.com/user-attachments/assets/15fcf170-39a9-4415-8d6c-7c83d1253279" />

---

En el código se puede apreciar que en primera instancia se trata de un simple punto que, dentro de su clase hay una función llamada ```step()``` que es la que se encarga de decidir la próxima dirección que va a tomar el punto dentro del sistema, para luego dibujarlo en cada fotograma de la ejecución del código

---
## Actividad 3, Distribuciones de probabilidad

### ¿Cuál es la diferencia entre una ditribución uniforme y gaussiana?

Con mis propias palabras se podría decir que una distribución uniforme es aquella cuyos datos tienen la misma posibilidad de ocurrir en todos los momentos del tiempo, ganandose el nombre de ser uniforme, por otra parte, la distribución gaussiana posee una probabilidad de que una media de datos tenga mayoe oportunidad de ocurrir que otra y muchas veces esas probabilidades de datos pueden llegar a desviarse más o menos de la media por una desviación estandar

### Código modificado para caminata con mayor probabilidad hacia la derecha

---
#### Resultado
<img width="646" height="242" alt="image" src="https://github.com/user-attachments/assets/48b11619-b5fc-4765-bb43-e2d32f25b841" />

---

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
  if (walker.x > width){
    walker.x = 0
  }
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(100, randomGaussian(255, 50), 100 );
    fill(100, randomGaussian(255, 50), 100 );
    circle(this.x, this.y, random(20,50));
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0 || choice == 1) {
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

## Actividad 4, Distribución normal

```js
let movingLine;

function setup() {
  createCanvas(600, 600);
  background(220);
  movingLine = new MovingLine
}

function draw() {

  
  movingLine.update();
  movingLine.display();
}

class MovingLine{
  constructor(){
    this.x = width / 2;
    this.y = height / 2;
    this.large = 0
    this.speed = 0.02;
    this.angle = 0;
  }

  update(){
    this.angle += this.speed;
    this.large = randomGaussian(250, 10);
  }

  display(){
    let x2 = this.x + this.large * cos(this.angle)
    let y2 = this.y + this.large * sin(this.angle)

    stroke(255, randomGaussian(142, 60), 40, 30);
    strokeWeight(3);
    line (this.x, this.y, x2, y2);

  }
  
}
```
---

### Resultado 

<img width="606" height="606" alt="image" src="https://github.com/user-attachments/assets/9a04a14a-4bcd-4e6b-89b0-3a6a8371d09d" />



---

En el resultado se puede ver como es una especie de linea que se va dibujando generando un círculo mientras se observa una linea mayormente naranjada que es la que se encarga de dibujar el cículo con algunas variaciones y es ahí donde entra la distribución normal puesto que el largo de las lineas está determinado por una distribución normal con una media de 250 y una desviación estandar de 10, lo que en gráfica se vería algo de este estilo

---

<img width="940" height="547" alt="image" src="https://github.com/user-attachments/assets/7f45ab53-f763-48c7-83e3-fe95d88dab26" />


## Actividad 5 


```js
let walker;

function setup() {
  createCanvas(600, 600);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();

  // Mantener dentro del canvas
  if (walker.x > width) walker.x = 0;
  if (walker.x < 0) walker.x = width;

  if (walker.y > height) walker.y = 0;
  if (walker.y < 0) walker.y = height;
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.r = 0;

    this.type = 1;

    this.change = millis() + random(1000, 5000);
  }

  show() {
    noStroke();

    if (this.type == 0) {
      fill(randomGaussian(255, 50), 140, 0, 60);
      this.r = random (5,10);
    } else if (this.type == 1) {
      fill(100, randomGaussian(255, 50), 100,);
      this.r = random (1,5);
    } else {
      fill(0, 100, randomGaussian(255, 50),40);
      this.r = random (20,15);
    }

    circle(this.x, this.y, this.r);
  }

  step() {

    if (millis() > this.change) {

      this.type = (this.type + 1) % 3;

      if (this.type == 0) {
        this.x = 0;
        this.y = 0;
      } else if (this.type == 1) {
        this.x = width / 2;
        this.y = height / 2;
      } else {
        this.x = 224;
        this.y = 150;
      }

      this.change = millis() + random(1000, 5000);
    }

    let step = 5;

    let xstep = walk() * step;
    if (random([false, true])) xstep *= -1;

    let ystep = walk() * step;
    if (random([false, true])) ystep *= -1;

    this.x += xstep;
    this.y += ystep;
  }
}

function walk() {
  while (true) {
    let r1 = random(1);
    let r2 = random(1);

    let probability = r1 * r1;

    if (r2 < probability) {
      return r1;
    }
  }
}
```
### Evidencias

<img width="601" height="598" alt="image" src="https://github.com/user-attachments/assets/f8fb7801-122c-4dcb-afbd-082a739bc530" />




## Reto de diseño

Empecemos por la idea, inicialmente para la idea del festival de ciencia y creatividad la idea principal era hacerlo algo parecido al espacio, pero por miedo a la funa por lo tradicional de la IA lo pensé un poco más y debido a ciertos contextos actuales del país, decidí optar por algo más relacionado a los ecosistemas y los biomas y es acá en donde empecé a pensar en el **[Ocean Fest](https://oceanfest.es/#festival)** el cual en palabras de ellos es el Festival del Mar: Arte, Ciencia, Talleres y Música, cuyo objetivo es invita a redescubrir la riqueza, la fragilidad y la importancia del océano promoviendo una relación más consciente con el entorno marino y fomentando el compromiso con su conservación, por lo que con base a esto, tomé como idea el hacer una experiencia que pudiera servir de primera vista a la hora de tener este festival.

### El objetivo de la experiencia
Lo que me gustaría buscar con esta experiencia es mostrar los ciclos de vida, la evolución y muestra de la diversidad que se encuentra en los biomas marinos y como la cantidad de vidas que hay dentro de espacios tan pequeños como pueden ser en 10 o 20 metros cuadrados pueden albergar grandes cantidades de vida que pueden nacer, existir y morir en pequeños instantes de tiempo sin que nosotros nos demos cuenta de eso.

### Primeros experimentos

Inicialmente quería experimentar con los modelos probabilísticos que iba a usar para tener una idea en general de la experiencia antes de adicionar detalles que pudieran hacer la experiencia más bonita y atractiva buscando en todo momento la naturalidad y los comportamientos emergente de lo que en el momento uno iban a ser meras partículas, pensando que en algún punto se volverían peces y organismos un poco más estilizados (eso lo resolveré en el futuro), por lo que por ahora, me dignaré a explicar los modelos de probabilidad que me gustaría utilizar en mi experiencia:

- **Urna de Pólya:** Realicé una pequeña investigación con IA de modelos probabilísticos interesantes y llegué a este, investigando un poco más por mi cuenta, este modelo suele tener cierta utilidad para generar hábitos en sistemas que tengan estos modelos, por lo que me puede servir para el movimiento de los peces en donde los movimientos repetidos pueden llegar a convertirse en tendencias, cosa que le pedí ayuda nuevamente a la IA para hacer una urna con un movimiento que se sintiera natural.

Despues de pensarlo, decidí que esta efectivamente si sirviera en los pesos de las direcciones que iban a tomar las entidades en el sistema por lo que, cada vez que una entidad eligiera una dirección, el peso de la decisión aumenta por lo que la hace más probable a repetirla después.

- **Distribuciones Gaussianas:** El uso que le voy a dar a esta va a ser un poco más sencilla ya que esta va a ser la encargada de generar esos desvíos raros que ocurren cuando ya hay un comportamiento esperado que nace a base de la **Urna de Pólya** donde son esos valores raros los que están más lejos de la media que los comportamientos habituales.

- **Distribuciones de Lévy:** Este se va a encargar de la distancia de los desplazamientos, siendo normalmente pequeños pero, que cada cierto tiempo hay saltos grandes. Sin embargo, quiero recalcar algo, el salto de levy que vimos en el libro de **Nature of Code** es un poco más sencilla y la IA me hizo la recomendación de "complicar" un poco la probabilidad del vuelo de Lévy por medio de una cola pesada, en donde ya no fuera solo una posibilidad u otra, si no que en su lugar existieran probabilidades intermedias, lo que, a largo plazo me ayudó a que el comportamiento de las partículas fuera más natural.

- **Probabilidades de Evoluciones e Involuciones:** Esto es para más adelante, pero estas son dos probabilidades que ocurren específicamente cuando hay dos organismos del mismo nivel y uno está lo suficientemente cerca del otro, lo que acciona la probabilidad de que exista una fusión y se muestre una especie más exótica, por el contrario, la probabilidad de involución ocurre de forma muy baja pero a cualquier organismo, lo que hace que "involucione", haciendo que las evoluciones no sean del todo permanentes y dependan de la probabilidad (esto no está implementado en el primer prototipo)
> Nota: luego recurrí a la idea de que las involuciones también se encarguen de que, esto sea una especie de costo por hacer saltos grandes en la exploración del ecosistema.
  
Luego de esto, ya me sentía satisfecho con las probabilidades que iba a usar y con eso procedí a hacer el primer prototipo y fue algo de este estilo.


https://github.com/user-attachments/assets/9d5f7d45-b835-42f6-bf92-6d9ed7cb7ba9


Para ser un primer prototipo debo admitir que si empecé a cuestionarme ciertas decisiones creativas, pero como se trataba de un primer prototipo iba a esperar a avanzar a una etapa más tardía para ver si debía tomar otro tema u otra decisión creativa, pero luego analizando un poco más la experiencia me dí cuenta que había un aspecto que me había gustado mucho y eso era el movimiento de las partículas, por lo que acá entró el concepto de las evoluciones y volatilmente entró el concepto de los imanes con el mouse, la cual fue una idea que descarté a medias porque, no me gustaba la idea de alterar completamente el movimiento de las prtículas con el mouse así que decidí que en lugar de que se utilizara para ser de imán, el mouse se encargaría de aumentar la probabilidad de sutilmente alterar el movimiento de las partículas más no iba a hacer que estas obligatoriamente cambiaran de movimiento, y luego de esto si pensé en las evoluciones, pero estos serían sistemas visualmente básicos lo cual fue un problema que me encargaría de resolver posteriormente ya que, actualmente lo que más me interesaba en este momento era verificar que los movimientos y los sistemas de probabilidad fueran óptimos, y que las evoluciones funcionaran como debían. por lo que procedí con otro prototipado para una versión dos para probar el sistema de evoluciones, resultando en lo siguiente:




https://github.com/user-attachments/assets/4b97beb8-eafc-4fa4-8173-e294a7e47742




Acá me gustaba un poco más la orientación que estaba tomando la idea, ya que con los modelos descartados de los sistemas planetarios que quería hacer no fueron lo suficientemente interesantes como para resaltar algo más (dejo evidencias de los modelos) ya que muchos se concentraban en unicamente crear sistemas que eran muy sencillo y creo que podrían haber llegado a sentirse un poco más vacíos.

<img width="1185" height="745" alt="image" src="https://github.com/user-attachments/assets/dbd4fd90-c6f6-40e1-9209-4ae4843718cf" />

Aparentemente es el único modelo guardado que tengo y no sé que pasó con el otro que tenía, creo que lo borré del estrés pero bueno, ya casi terminando, ahora tocaba el apartado visual y un pequeño cambio que quería hacer en el sistema, ya que, a pesar de que la experiencia funciona con el mouse, no me llamaba lo suficiente así que decidí ponerme las botas de experiencias y hacer interacciones utilizando Mediapipe por medio de un modelo de Machine Learning de una biblioteca llamada **[ml5.js](https://docs.ml5js.org/#/reference/handpose)** con la cual haría que se pudiera controlar la literna que en un principio se movía con el mouse, ahora podía hacerse con una de las manos, y que en adición a lo anterior cada vez que se tocaran los dedos índice y pulgar, se crearan nuevos organismos de bajo nivel. por lo que, después de solicitar a la IA que quería hacer de la experiencia un ecosistema acuatico con evoluciones notorias entre niveles (peces pequeños, peces medianos, peces grandes, mantarrayas y leviatanes), además de un fondo medio notorio y una linterna para enfocar la posición de donde está el trackeo de las manos para que no se viese un círculo incípido, se terminó con el siguiente prototipo y resultado:


https://github.com/user-attachments/assets/75e4bd35-0636-476c-a0cf-d32fd8ea93ec



https://editor.p5js.org/KiwisCas/full/YgXDqIcZI


Pero esto no es todo, ya que nuevamente estré en pánico y no sentía mucho atractivo visual así que, pensé agregarle unas cuantas animaciones a las evoluciones e involuciones, unos cuantos cambios a las formas ya que algunas me parcían un poco básicas y variaciones y dinamismo para sentir una mejor narrativa de la experiencia, así que, espcificando estos detalles de diseño e interacciones nuevamente a una IA, solicité que me ayudara con este proceso, manteniendo la estructura de modelos de probabilidad ya que como comenté, me gustó el resultado que obtuve en el diseño de la arquitectura, este sería el resultado final.

https://editor.p5js.org/KiwisCas/full/M5zG_N9xT




https://github.com/user-attachments/assets/41d8b4db-b937-49d2-a627-a87a87265c4d

