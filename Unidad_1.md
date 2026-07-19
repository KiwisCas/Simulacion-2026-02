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
## Actividad 2, La aleatoriedad en el arte generativo
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
## Actividad 2, Distribuciones de probabilidad





