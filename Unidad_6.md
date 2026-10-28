# Bitácora de inspiración: Physarum Live Performance

## Cómo llegué a la idea

Mi historia con esta idea nace buscando una canción con una estética más acuática. No quería hacer algo demasiado electrónico porque el tema de los Physarums ya tenía una parte muy técnica y algorítmica. Quería encontrar una canción que me ayudara a llevar el proyecto hacia un lugar más suave, atmosférico y relacionado con el agua.

En medio de una clase de cine fantástico, Tomy me habló de una canción que tenía unas vibras **Frutiger Aero**. La canción se llamaba **“So Good” de Jhené Aiko**. Cuando la escuché, me gustaron mucho las vibras que tenía y decidí que esa iba a ser la canción de mi proyecto.

Como el tema que estaba buscando era algo acuático, también quería que la visualización hiciera una alusión directa a ese mundo. Por eso puse una ballenita como uno de los *points* del Physarum. No quería que fuera solamente una figura decorativa, sino una forma que pudiera contener a las partículas y convivir con su movimiento.

Así fue como la canción terminó definiendo la dirección visual del proyecto. La ballena, los colores azul y verde claro y el movimiento de los agentes empezaron a formar una estética inspirada en el agua, pero también en esa sensación digital y brillante de Frutiger Aero.

## Referencias visuales

Antes de decidir cómo quería que funcionara el instrumento, revisé dos referencias que me ayudaron a pensar la relación entre movimiento, interacción y atmósfera:

- [Bleuje: explicación de Physarum](https://bleuje.com/physarum-explanation/), para entender cómo los agentes pueden construir formas a partir de sensores, rastros, depósito y difusión.
- [Sage Jenson: 36 Points](https://www.sagejenson.com/36points/), para observar una experiencia en la que diferentes teclas activan personalidades y comportamientos paramétricos.

De la referencia de Bleuje tomé la idea de que una forma puede aparecer como consecuencia de reglas simples, en lugar de estar dibujada desde el comienzo. De Sage Jenson tomé la inspiración para convertir los puntos en cambios de comportamiento y no solamente en botones o filtros.

Durante la exploración visual registré dos momentos importantes. En la primera captura observé la composición de **36 Points** y la manera en que los puntos se presentan como distintas posibilidades de movimiento. En la segunda captura observé mi propio prototipo con la ballena construida por partículas, ya rotada en el plano 2D y con los agentes formando la silueta.

Estas capturas funcionaron como referencias de proceso: una muestra el punto de partida conceptual y la otra muestra cómo traduje esas ideas a mi propio instrumento. No quise copiar el código, la identidad visual ni los recursos de los sitios; utilicé sus principios como inspiración para desarrollar una pieza diferente, relacionada con mi canción y con la estética acuática que estaba buscando.

## Qué perciben los agentes

Elegí trabajar con Physarum porque me interesaba que los agentes pudieran construir la imagen entre todos, sin que yo tuviera que dibujar cada trayectoria.

Cada agente percibe la intensidad del rastro que existe:

- delante de su dirección actual;
- hacia su lado izquierdo;
- hacia su lado derecho;
- en su posición local.

Con esa información compara los tres sensores y decide si continúa, gira a la izquierda o gira a la derecha. Después avanza, deposita una señal y esa señal se difunde y desaparece gradualmente.

Los agentes tienen límites claros. No conocen la composición completa ni siguen una trayectoria predeterminada. Solo pueden leer una zona cercana, moverse dentro del espacio normalizado de la pantalla y reaccionar a los parámetros que les comparto. En el modo ballena, además, reciben una atracción suave hacia la geometría, pero siguen calculando su movimiento mediante las reglas de Physarum.

La simulación utiliza:

- sensores en tres direcciones;
- adaptación a la densidad local;
- rotación según los rastros detectados;
- avance con una distancia determinada;
- depósito sobre el mapa de trails;
- difusión y decaimiento del rastro;
- wrapping en los bordes.

## La línea diagonal

En una de las pruebas apareció una línea diagonal que no quería. Era demasiado rígida y no tenía la sensación orgánica que estaba buscando. Revisé los estados, los shaders y la textura de posiciones.

Encontré una declaración duplicada de `texturePosition` en el shader de cómputo y una lectura incorrecta de las coordenadas de las partículas. Eliminé la duplicación, añadí el atributo `particleUv` y conecté correctamente la variable de posiciones consigo misma:

```js
gpuCompute.setVariableDependencies(particleVariable, [particleVariable]);
```

Después probé los estados `1`, `2`, `3` y `4`. Cada uno empezó a producir un comportamiento diferente y la línea diagonal desapareció. El movimiento recuperó sus irregularidades, sus agrupaciones y sus desvíos.

## Por qué elegí Physarum

Elegí Physarum porque sus reglas se relacionan con la idea de una corriente viva. Cada agente reacciona a las huellas de los demás, y de esa interacción aparece una estructura que no está dibujada directamente.

La combinación de sensado, giro, avance, depósito y difusión produce comportamientos emergentes. Yo puedo cambiar los parámetros comunes, pero no controlo la ruta exacta de cada agente. Esto permite que la interpretación tenga una parte humana y otra parte impredecible.

No utilicé steering behaviors, flocking ni flow fields como algoritmos principales. La propuesta se construye únicamente con Physarum, cumpliendo la restricción del encargo. La geometría de la ballena funciona como un entorno de contención y no como un sistema de movimiento separado.

## Treinta y seis personalidades

Quise que el instrumento tuviera diferentes maneras de reaccionar. Por eso incorporé 36 personalidades Physarum asociadas a las teclas numéricas y alfabéticas.

Cada punto modifica una combinación de:

- distancia de los sensores;
- ángulo de los sensores;
- ángulo de rotación;
- velocidad de movimiento;
- fuerza de atracción;
- adaptación a la densidad;
- cantidad de depósito;
- decaimiento del trail;
- brillo.

No son solamente filtros de color. Cada tecla cambia la forma en que los agentes perciben y calculan sus acciones. Algunos estados generan filamentos más finos, otros producen redes más densas, movimientos más caóticos o agrupaciones más atraídas.

Estos controles me permiten conducir el sistema en vivo. Puedo sostener un estado si la canción necesita continuidad o cambiar de punto cuando quiero que la imagen se abra, se acumule o se vuelva más inquieta.

## La ballena como point

Como la canción me llevó a pensar en algo acuático, necesitaba que esa intención apareciera dentro del instrumento. Por eso incorporé `whale.glb` como uno de los *points* principales.

Recorrí los vértices del modelo, normalicé sus coordenadas y los convertí en una `DataTexture` de posiciones objetivo. Al activar la tecla `W`, las partículas continúan calculando su respuesta Physarum, pero también reciben una fuerza de contención que las atrae hacia la silueta.

La ballena no es una imagen fija. Es un recipiente vivo. Las partículas se mueven dentro de la forma, se separan, regresan y deforman el contorno. Añadí una ondulación temporal del cuerpo y una animación más marcada en la cola para que la figura pareciera nadar.

La geometría apareció inicialmente invertida verticalmente. Corregí la coordenada `y` y después giré la ballena 90 grados dentro del plano 2D, alrededor del centro del lienzo:

```js
const WHALE_ROTATION_2D = Math.PI * 0.5;
```

Con esto la ballena pudo verse desde la orientación que quería sin convertirse en una rotación tridimensional.

## La música y la interpretación humana

La canción audible que elegí está dentro de la carpeta `assets`:

```text
assets/Jhené Aiko - So Good feat. Kendrick Lamar (Official Audio).wav
```

También preparé una pista vocal separada para realizar un análisis discreto:

```text
assets/music/Vocals.mp3
```

El análisis vocal no es el conductor principal de la obra. Lo uso como una influencia secundaria para producir pequeñas variaciones de velocidad, leves cambios de dirección y pulsos discretos de azul claro o verde claro.

La conducción principal la hago yo durante la ejecución. Escucho la canción, observo la respuesta del sistema y decido qué punto activar, cuándo usar la ballena, cuándo sostener una forma y cuándo generar un beat. El análisis de audio acompaña la imagen, pero no reemplaza mis decisiones.

Esto es importante para el encargo: no quiero que el sistema reproduzca una secuencia automática que ya esté decidida. Quiero que exista una interpretación en vivo, donde la canción orienta mis decisiones y los agentes responden de forma emergente.

## El beat y la tecla G

Añadí un botón **Beat** y asigné la tecla `G` a la misma acción. Cuando activo el beat, las partículas reciben un pequeño impulso lateral, una variación leve de velocidad y una alteración suave de dirección.

También aumenta discretamente la iluminación y el tamaño de algunos puntos. El pulso se desvanece durante unos segundos, por lo que no funciona como un destello aislado, sino como una onda que atraviesa la composición.

El beat es uno de mis controles expresivos principales. Me permite responder a un momento de la canción sin abandonar el comportamiento autónomo de los agentes.

## Controles expresivos

Durante la presentación utilizo pocos controles, pero cada uno tiene una consecuencia visible:

| Control | Acción | Consecuencia visual |
|---|---|---|
| `1` a `4` | Cambiar estado base | Modifica la densidad, velocidad y forma del movimiento |
| `0` a `9`, `A` a `Z` | Cambiar personalidad | Cambia la percepción y las reglas de cada agente |
| `W` | Activar la ballena | Atrae las partículas hacia la geometría acuática |
| `G` | Activar beat | Genera un salto leve y un pulso de iluminación |
| `H` | Ocultar interfaz | Deja la pantalla completa para la presentación |
| Click y arrastre | Mover el atractor | Orienta localmente a los agentes hacia el cursor |

Estos controles no cambian solamente la apariencia. Cambian el entorno, los parámetros compartidos o la forma en que los agentes calculan sus acciones.

## Score visual para la interpretación

El score visual no es una secuencia automática. Es una guía para escuchar la canción y decidir cuándo intervenir.

### Introducción

Comienzo con un estado de movimiento fino y poca intervención. Dejo que los agentes construyan los primeros rastros y observo cómo aparece la imagen.

**Intención:** presentar el espacio acuático lentamente, como si la pantalla empezara a llenarse de agua.

**Intervención:** utilizar `1` o una personalidad de baja velocidad y mantener el movimiento sin activar todavía la ballena.

### Entrada de la voz

Cuando la voz empieza a tomar presencia, busco una organización más visible. Cambio a un punto con mayor densidad o sensores más amplios.

**Intención:** hacer que las partículas parezcan responder a una corriente vocal sin convertir la respuesta en un efecto obvio.

**Intervención:** cambiar de personalidad y observar la variación del rastro. Mantener el estado si la forma encuentra un equilibrio.

### Desarrollo de la canción

En la parte central permito que la red se vuelva más orgánica. Puedo activar el contenedor de la ballena para que la abstracción se convierta en una imagen reconocible.

**Intención:** relacionar directamente la canción con el agua y mostrar la ballena como una criatura formada por partículas.

**Intervención:** pulsar `W`, esperar a que la silueta se construya y sostenerla mientras observo el movimiento de la cola y del cuerpo.

### Momento de mayor energía

Cuando siento que la canción alcanza un momento de mayor intensidad, utilizo un punto con más velocidad o mayor atracción. En ese momento puedo activar `G`.

**Intención:** producir un salto breve de la materia, como una respiración más profunda o un movimiento de la ballena bajo el agua.

**Intervención:** pulsar `G` una vez, no repetirlo innecesariamente y dejar que el pulso se desvanezca durante los siguientes segundos.

### Parte de calma

Cuando la canción vuelve a sentirse más contenida, cambio a una personalidad con menor velocidad y menos depósito.

**Intención:** dejar que los rastros se disuelvan y que la imagen tenga espacio para respirar.

**Intervención:** sostener un estado, mover suavemente el cursor o desactivar la ballena con `W` si necesito regresar a una forma más abstracta.

### Cierre

Termino con una composición estable, sin intentar controlar cada partícula. Oculto la interfaz con `H` para que la última imagen quede limpia y la atención se concentre en la relación entre la música, la ballena y los rastros.

## Preparación y ensayo

Para preparar la presentación debo comprobar:

- que la canción principal cargue desde `assets`;
- que `Vocals.mp3` se analice sin reproducirse;
- que el sistema funcione mediante un servidor local o GitHub Pages;
- que las teclas `1` a `4` cambien los estados;
- que las 36 personalidades sean accesibles;
- que `W` active y desactive la ballena;
- que `G` genere el beat;
- que `H` oculte la interfaz y el cursor;
- que el click y el arrastre modifiquen el atractor;
- que la simulación conserve un rendimiento adecuado en pantalla completa.

Durante el ensayo debo evitar seguir el score de manera rígida. El score me orienta, pero la interpretación depende de lo que escuche y de lo que esté ocurriendo en la pantalla.

## Autoevaluación

### 1. Cumplimiento del encargo — 23/25

Considero que mi instrumento cumple con la propuesta porque funciona en la Web, utiliza Three.js y GPUComputationRenderer, trabaja en tiempo real y permite interpretar la canción elegida.

La visualización tiene controles para intervenir durante la ejecución y puede utilizarse en pantalla completa mediante el modo presentación. La principal consideración es que el audio debe cargarse desde un servidor local o desde GitHub Pages, porque el navegador puede bloquear recursos al abrir directamente con `file:///`.

### 2. Comprensión y verificación — 23/25

Puedo explicar qué perciben los agentes, cuáles son sus límites y cómo calculan sus acciones. También puedo explicar cómo se construyen los trails, cómo funciona la textura de posiciones y cómo la fuerza de contención aproxima las partículas a la ballena.

Verifiqué los estados `1`, `2`, `3` y `4`, la desaparición de la línea diagonal, la rotación 2D de la ballena, la activación de `W`, el beat con `G` y la carga separada de `Vocals.mp3`.

### 3. Diseño e intención — 24/25

La selección de Physarum se relaciona con mi intención de construir una estética acuática, orgánica y emergente. La canción “So Good” fue el punto de partida para buscar una atmósfera Frutiger Aero y la ballena traduce esa idea acuática dentro del instrumento.

Los colores azul y verde claro, la fluidez de los agentes y la deformación de la ballena sostienen una misma dirección visual. La canción no está usada solamente como fondo: guía el ritmo y la atmósfera de mis decisiones.

### 4. Interpretación humana — 22/25

El score visual organiza posibles intervenciones, pero no obliga a seguir una secuencia automática. Yo decido cuándo cambiar de personalidad, activar la ballena, usar el beat o dejar que el sistema continúe en un estado.

Los controles tienen consecuencias perceptibles y permiten responder a la canción. Considero que todavía debo ensayar más la coordinación entre escucha, observación y cambio de parámetros para que la interpretación sea más segura.

### Total

**92/100**

Conversión: **4.6/5**

## Página web

https://kiwiscas.github.io/Unidad6Physarum/

## Estado final de la propuesta

Mi instrumento visual parte de una canción que descubrí gracias a una conversación durante una clase de cine fantástico. A partir de sus vibras Frutiger Aero construí una experiencia acuática con Physarum, donde los agentes perciben rastros cercanos, calculan sus propios movimientos y generan formas emergentes.

La ballena es el punto donde mi idea musical y mi algoritmo se encuentran. No es solamente una figura: es una forma acuática que las partículas intentan recordar mientras siguen moviéndose.

La interpretación final depende de la relación entre tres cosas: lo que escucho, las decisiones que tomo y la respuesta impredecible de los agentes. Esa relación es la que convierte la simulación en un instrumento visual.
