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
