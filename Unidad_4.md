# Unidad 4 Los encargos de diseño
## Inicios de idea
> No se me ocurre nada

> Actualización: ya sí


### Sobre la idea

Bienvenidos a un tutorial en youtube de como inicialmente no sabía que hacer pero ahora si


En un principio recordé en que me basaba para hacer algunos de mis trabajos en sistemas físicos imnteractivos uno y como algunos de estos acababan siendo un enredo completo o un licuado de ideas que inicialmente tenía planeado y por temas de viabilidad acababa realizando y no me sentía muy satisfecho con el resultado, pero ahora, ya un año despues (creo) decidí volver a hacer algo que quería hacer durante estos semestres y era hacer un trabajo con estética retro, lo cual se que no es mucho pero me gustaría hacerlo


### Desarrollo

Empecé Bajo la idea de querer hacer algo un poco sencillo pero que me gustaba la idea, recuerdo que en steam hay una app en donde puedes ponerle fondos de pantalla animados a tu computador, así que pensé en hacer este sistema lo más parecido a un fondo de pantalla en movimiento pero con vida y sonido siguiendo las bases del modelo de kuramoto así que eso empecé a hacer, empecé investigando referentes sobre algo de estética retro y volví a redescubrir el tema de los synthwaves:

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/9bd8cd56-38ee-437b-9e5d-afc74905f50e" />

Así que decidí basar mi fondo de pantalla interactivo pura y meramente en eso, los synthwaves, teniendo como agentes pricipales 4 y siendo complementarios otros 4, cada uno siendo los elementos más básicos de lo que es una synthwave, un sol, un oceano y una especie de montañas, incialmente me gustaba la idea hacer una especie de ciudad retro que progresivamente fuera aumentando en polígonos hasta quew se volviera super realista, así que empecé con una base para la IA para ver que pasaba y...



https://github.com/user-attachments/assets/fcaf72e9-2205-4ee0-a2a3-7f4a38ba4d91


### Uy, así no era


Pero vale huevo, ya tengo una base de lo que **No debo hacer JAJAJAJAJ**

Inicialmente empecé a hacer cambios y empecé con esta base

<img width="1185" height="297" alt="image" src="https://github.com/user-attachments/assets/f7d9a397-88a8-4319-b4e2-00ebc4fece18" />

La cual no me convence para comenzar el trabajo pero el hecho de que ya tenga una serie de triángulos para saber como puede funcionar el océano y nació la base del proyecto ahora si


<img width="1014" height="789" alt="image" src="https://github.com/user-attachments/assets/c030c9b4-60c6-4309-b787-d54faf685c80" />

Como puedes ver, no hay mucho pero por lo menos hay una pequeña base para donde comenzar, me gustaba la estetica así que definitivamente opté por continuar con el estilo de synthwave más con lo que se veía en la imagen de referencia 


Pero bueno, esto si fue un mejor referente para empezar con lo que respecta este proyecto, por lo que opté por empezar a promptear y llegue a esto

<img width="1185" height="937" alt="image" src="https://github.com/user-attachments/assets/efad288e-471b-4e34-a405-4dc849196322" />






## Explicación técnica de la implementación
Extensión del modelo para los demás agentes

Para que el sol, el planeta, las montañas, el océano, el cielo, el glitch, las naves y los rectángulos no fueran simples decoraciones sincronizadas por un reloj externo, se añadió una extensión de campo medio.

Cada agente auxiliar tiene su propia fase y frecuencia natural, y se actualiza con:


$$\frac{d\theta_a}{dt} = \omega_a + K\sin(\bar{\theta}-\theta_a)$$


Aquí $\bar{\theta}$ es la fase media de los ocho diamantes. Esta extensión conserva la estructura de Kuramoto, pero reemplaza la suma completa de interacciones por una influencia colectiva resumida. La decisión busca conectar las personalidades audiovisuales con la población principal sin convertir cada elemento de la escena en una nueva población independiente.

La implementación es:

```js
function advanceAuxiliaryKuramotoAgents(dt) {
    let meanSin = 0;
    let meanCos = 0;

    diamondAgents.forEach(agent => {
        meanSin += Math.sin(agent.theta);
        meanCos += Math.cos(agent.theta);
    });

    const meanPhase = Math.atan2(meanSin, meanCos);
    const auxiliaryAgents = [
        backgroundAgent,
        sunAgent,
        planetAgent1,
        oceanAgent,
        glitchAgent,
        ...horizonAgent.mountainAgents,
        ...shuttles,
        ...shipAgents,
        ...visualizerBars
    ];

    auxiliaryAgents.forEach(agent => {
        const previousTheta = agent.theta;
        const dTheta = agent.omega
            + K * Math.sin(meanPhase - agent.theta);

        agent.theta = (agent.theta + dTheta * dt)
            % (Math.PI * 2);

        agent.phasePulse = agent.theta < previousTheta
            || (previousTheta < Math.PI
                && agent.theta >= Math.PI);
    });
}
```

La extensión modifica la topología de interacción: los diamantes tienen interacción todos-con-todos y los agentes auxiliares reciben el campo medio de la población principal. El objetivo es que el paisaje completo siga la organización de los diamantes, pero conserve personalidades y frecuencias propias.

## 5. Personalidades audiovisuales

### 5.1 Diamantes: percusión cristalina

- Forma: octaedros.
- Movimiento: desplazamiento sobre el océano, salto y aterrizaje.
- Sonido: nota aguda, pluck triangular o acorde cuando coinciden varios aterrizajes.
- Respuesta: cada aterrizaje genera una onda, un destello y una nota.
- Relación con Kuramoto: el salto ocurre cuando la fase alcanza una zona determinada; la coincidencia temporal de varios saltos produce armonización.

```js
if (!this.hasJumpedInCycle &&
    Math.abs(this.theta - umbralSalto) < 0.25 &&
    this.isGrounded) {
    this.vy = 2.5 + orderParameter * 6;
}
```

### 5.2 Montañas: subgrave y deformación lenta

- Forma: siluetas montañosas.
- Movimiento: variación de altura según `sin(theta)`.
- Sonido: subgrave y cuerpo triangular al alcanzar un pico.
- Respuesta: el contorno cambia de brillo y emite un evento de baja frecuencia.

```js
const sineVal = Math.sin(agent.theta);
agent.currentH = agent.baseH *
    (1.0 + sineVal * 0.22 * (0.5 + orderParam * 0.5));

const atPeak = sineVal > 0.94;
if (atPeak && !agent.lastPeak) {
    audioSystem.triggerMountainPeak(agent.currentH, orderParam);
}
```

### 5.3 Naves: impulso filtrado y trayectoria

- Forma: naves triangulares con estelas.
- Movimiento: desplazamiento espacial e impulsos.
- Sonido: onda de diente de sierra filtrada.
- Respuesta: la fase produce impulsos, cambios de inclinación, brillo y ondas en el océano.

```js
if (this.phasePulse) {
    this.triggerImpulse(orderParameter);
    this.phasePulse = false;
}
```

### 5.4 Sol, planeta y atmósfera: estados globales

- Sol: pulso de luz y bajo de `Bass.wav` cuando cruza fase.
- Planeta: rotación, escala, pulso visual y tono grave.
- Cielo: brillo, color y resplandor de horizonte dependientes de la fase y de `r`.
- Glitch: ruido y distorsión que aumentan en el desorden.

Estas entidades no son solamente cambios de color. Sus eventos, intensidad, movimiento, textura y sonido dependen de las fases y del estado colectivo.

### 5.5 Rectángulos: barras resonantes

Los `VisualizerBar` funcionan como barras laterales. Tienen fase y frecuencia, y al cruzar su fase generan rebote y escala vertical. También pueden responder a las notas de los diamantes mediante `triggerSideBarBounce`.

```js
if (this.phasePulse) {
    this.triggerBounce();
    this.phasePulse = false;
}

const phaseScale = 1.0 +
    Math.max(0, Math.sin(this.theta)) * 0.25;
```

## 6. Sonido como consecuencia del modelo

El audio no está organizado como una secuencia fija de eventos. Hay tres relaciones principales:

1. El aterrizaje de un diamante depende de su fase.
2. Varios aterrizajes cercanos en el tiempo se agrupan y producen un acorde.
3. El valor de `orderParameter` cambia el comportamiento sonoro global: en desorden se usan disparos de bajo y ruido; en armonía aparece un bajo continuo con pulsos coordinados.

El agrupamiento de notas en una ventana de 35 milisegundos no genera el comportamiento de Kuramoto. Solo permite que eventos que ocurren casi simultáneamente se perciban como una unidad musical.

## 7. Interacción performativa

La experiencia se puede tocar mediante varias acciones:

### Intervención global

`Perturbar Todo` asigna una fase aleatoria a cada diamante. Esto reduce normalmente el orden colectivo y permite observar la reorganización posterior.

```js
diamondAgents.forEach(a => {
    a.theta = Math.random() * Math.PI * 2;
    a.hasJumpedInCycle = false;
});
```

`Reiniciar Fases` coloca todos los diamantes en la misma fase, permitiendo demostrar una configuración altamente organizada.

### Intervención individual

Al hacer clic sobre un diamante, se modifica únicamente su fase:

```js
diamondAgents[index].theta = Math.PI * 0.4;
diamondAgents[index].hasJumpedInCycle = false;
glitchAgent.triggerBurst(0.85);
```

Esto permite romper parcialmente una configuración estable y observar cómo el acoplamiento intenta reorganizar el colectivo.

### Configuración continua

El performer puede mover `K` y `sigma` durante la ejecución, además de intervenir con los botones y los clics. La combinación permite explorar:

1. `K` bajo o `sigma` alta: desorden.
2. `K` medio: organización parcial.
3. `K` alto y `sigma` baja: estabilidad.
4. Clic o perturbación global: ruptura temporal y reorganización.

## 8. Errores corregidos durante el desarrollo

El modo desarrollador utilizaba `samplerEngine.getStatus()`, pero el método no existía. Se añadió:

```js
getStatus() {
    return {
        loaded: Boolean(this.buffers.bass),
        duration: this.buffers.bass
            ? this.buffers.bass.duration.toFixed(2)
            : '0.00'
    };
}
```

También se eliminaron los temporizadores que controlaban directamente eventos principales del planeta, las naves y las montañas. Los `setTimeout` que permanecen se utilizan para agrupar eventos de audio o controlar la duración de efectos breves, no para sustituir la dinámica de fase.

Se verificó la sintaxis con:

```text
node --check main.js
```

Resultado: correcto.

## 9. Autoevaluación

La siguiente evaluación utiliza los cuatro criterios indicados para la presentación. La puntuación máxima es de 100 puntos.

| Criterio | Evidencia | Puntaje máximo | Autoevaluación |
|---|---|---:|---:|
| 1. Cumplimiento de requisitos mínimos | Hay 8 diamantes, varias personalidades audiovisuales, controles para `K` y `sigma`, interacción global e individual, perturbaciones y tres estados colectivos. | 25 | 24 |
| 2. Explicación de las variables de Kuramoto | `theta`, `omega`, `K`, `sigma`, `N` y `r` están definidos y relacionados con la implementación. | 25 | 24 |
| 3. Explicación de cómo las variables producen el comportamiento | Las fases controlan saltos, notas, pulsos, movimiento, brillo, glitch y deformación; se explica también la extensión de campo medio. | 25 | 23 |
| 4. Demostración de los objetivos de la unidad | La aplicación puede tocarse y permite pasar de desorden a organización y romper la estabilidad. La demostración depende de que el audio se inicie con una interacción y de que se expliquen las personalidades. | 25 | 23 |
| **Total** |  | **100** | **94** |


**Autoevaluación final: 94/100 = 4.7/5.0**


## Link de la página web

[Modelo de kuramoto](https://kiwiscas.github.io/Oscilaciones/)
