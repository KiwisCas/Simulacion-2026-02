# Unidad 2

## Actividad 1 
## Actividad 2

<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/c333a5a6-b8e3-4801-a0ee-89ab91bb258f" />


<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/2e94cbdf-f8bd-4ddb-baa5-ce2ac40e828b" />


## Actividad 4 y 5

### Intención

#### Tensión

**Quiero explorar la tensión entre el espíritu y las emociones**

La intención que tengo con este tema es buscar una forma de representar cómo ambas dimensiones coexisten en un estado de negociación permanente. El espríritu busca mantener quilibrio, estabilidad y contemplación, mientras que por otra parte las emociones suelen ser aspectos impulsivos, llenos de tensión al igual que de momentos de cambio.

No pretendo ante todo representar conceptos o religiosos o psicológicos de una forma absolutamente literal, por otra parte la contradicción se incorpora en las interacciones de las partículas, la persona que observe debería poder percibir ciclos donde el sistema logra percibirse en calma, acumulando tensión y reorganizandose, lo que modifica temporalmente su estructura antes de vovlerse a estabilizar

La idea es comprender que ese equilibrio al que aspiramos todos en todo momento no es algo fijo, si no que en su lugar es el resultado de muchas cosas (en el sentido literal fuerzas, en el sentido artístico hablamos de la estabilidad emocional) que continuamente dentro de si se ve afectada

### Operaciones de mentalidad

#### Intención

Me gustaría explorar como el esíritu y las emociones no funcionan como tal como opuestos absolutos, si no como unas fuerzas que constantemente negocian el comportamiento del sistema

Me gustaría hacer una experiencia en donde el equilibrio no sea algo permanente si no algo que emerge y que puede alterarse y volverse a contruir

#### Entidades

Me gustaría que el sistema tuviera 4 partículas o entidades
  - Azules (espíritu)
  - Rojas (emociones)
  - Verdes (mediadoras)
  - Amarillas (impulsos o cambio)
    
Donde cada una de estas partículas va a tener sus propias reglas de movimiento y relación respecto a las otras.

#### Relaciones

las relaciones se van a definir por medio de una matriz de atracción, así que inicialmente planeo adaptar una serie de relaciones asimétricas

Por ejemplo:

  - Las emociones buscan acercarse al espíritu.
  - El espíritu tiende a alejarse cuando las emociones aumentan su intensidad.
  - Las partículas verdes favorecen la cohesión general.
  - Las amarillas introducen exploración y rompen configuraciones demasiado estables.

Estas relaciones producen comportamientos conjuntos que no deberían poder llegar a ser predecibles simplemente observando

#### Entradas

El sistema va a recibir información de la siguiente forma:

- Distribuciones aleatorias al inicio
- Tiempo, mediante ruido Perlin que se va a encargar de modificar sutilmente algunas relaciones
- Interacciones del usuario mediante el mouse (Mover el cursor va a generar corrientes que arrastren partículas y un clic va a producir ondas que afecten el movimiento)


#### Reglas

Cada frame ejecuta el siguiente proceso:

  1. Actualización dinámica de la matriz de relaciones.
  2. Cálculo de fuerzas entre partículas cercanas.
  3. Acumulación de presión emocional.
  4. Explosiones cuando la presión supera un umbral.
  5. Aplicación de velocidad, aceleración y fricción.
  6. Reposicionamiento continuo mediante "wrap".
  7. Renderizado según el estado interno de cada partícula.

#### Invariantes

Estas cosas siempre van a quedar igual:

  - cuatro tipos de partículas;
  - las reglas generales de interacción;
  - la existencia de presión emocional;
  - las explosiones;
  - la presencia de relaciones asimétricas;
  - la interacción mediante el cursor;
  - la dinámica basada en distancia.

#### Variabilidad

Cada simulación va a tener resultados diferentes porque:

- Las posiciones iniciales son aleatorias
- Las velocidades iniciales son diferentes
- Hay fases individuales
- Hay evoluciones por medio de ruidos Perlin
- Hay diferentes momentos de explosión emocional
- Hay intervenciones del usuario

#### Reflexiones (después del desarrollo)

Durante las pruebas se observó que algunos comportamientos resultaban especialmente significativos.

Las explosiones emocionales producen reorganizaciones completas del sistema sin destruir su identidad.

La gran cantidad de partículas mediadoras evita que el sistema permanezca indefinidamente en estados caóticos.

También aparecieron agrupaciones inesperadas que no respondían directamente al concepto buscado. Estas configuraciones fueron consideradas accidentes interesantes únicamente cuando contribuían a hacer visible la tensión entre estabilidad y transformación.


### Diseño del sistema

#### Tipos de partículas

**Azules**

- Representan las emociones.
- Acumulan presión cuando interactúan entre sí.
- Al alcanzar un umbral generan explosiones que modifican el comportamiento de todas las partículas cercanas.
- Posteriormente atraviesan un período de recuperación.

**Seleccioné las partículas azules porque quiero hacer perceptible la búsqueda de equilibrio. Espero que produzcan regiones organizadas que resistan parcialmente las perturbaciones del sistema.**


**Rojas**

- Representan la dimensión espiritual.
- Poseen velocidades moderadas y alta fricción.
- Buscan conservar configuraciones relativamente estables.

**Seleccioné este comportamiento porque quiero hacer perceptible que las emociones pueden intensificarse, desbordarse y reorganizarse. Espero que produzca cambios repentinos sin romper definitivamente la estabilidad del sistema.**

**Verdes**

- Funcionan como reguladoras.
- Conectan continuamente distintas regiones del sistema.
- Reducen la fragmentación producida por las emociones.

**Seleccioné una cantidad considerablemente mayor porque quiero hacer perceptible que el equilibrio necesita estar presente de manera constante. Espero que produzcan reorganizaciones progresivas después de cada episodio emocional.**


**Amarillas**

- Representan impulso y exploración.
- Poseen la mayor velocidad.
- Introducen trayectorias nuevas que impiden estados completamente estáticos.

**Seleccioné estas partículas porque quiero hacer perceptible el cambio permanente. Espero que produzcan nuevas conexiones entre regiones del sistema sin convertirse en la fuerza dominante.**

#### Partículas

| Tipo                 | Cantidad | Justificación                                   |
| -------------------- | -------: | ----------------------------------------------- |
| Espíritu (Azules)    |       45 | Presencia constante pero no dominante.          |
| Emociones (Rojas)    |       45 | Peso equivalente al espíritu.                   |
| Mediadoras (Verdes)  |      140 | Mantienen la estabilidad global.                |
| Impulsos (Amarillas) |       40 | Introducen variabilidad sin dominar el sistema. |

#### Matriz de relaciones

<img width="376" height="368" alt="image" src="https://github.com/user-attachments/assets/9091ad3f-ad5d-433f-9088-006a6fcc460a" />

La matriz también cambia lentamente mediante ruido Perlin, permitiendo que el comportamiento colectivo evolucione sin perder identidad.


#### Intensidad y Alcance

Las interacciones se calculan únicamente cuando las partículas se encuentran dentro del radio de interacción.

Cuando la distancia es demasiado pequeña aparece una fuerte repulsión que evita el colapso del sistema.

Cada población posee además un pequeño movimiento tangencial que genera trayectorias curvas y evita movimientos excesivamente mecánicos.

#### Distancia de interacción

El radio de interacción se fijó en aproximadamente 90 píxeles.

Este valor permite formar agrupaciones locales sin convertir toda la simulación en un único comportamiento global.

**Seleccioné esta distancia porque quiero hacer perceptible que las relaciones tienen un alcance limitado. Espero que produzca múltiples regiones de organización que posteriormente interactúan entre sí.**

#### Fricción y velocidad máxima

- Cada población posee parámetros propios.

- Las partículas espirituales presentan mayor fricción.

- Las emocionales mantienen velocidades intermedias.

- Las amarillas alcanzan la mayor velocidad.

- Las diferencias generan ritmos distintos sin modificar continuamente las reglas principales.

#### Distribución inicial

Las partículas aparecen distribuidas aleatoriamente sobre toda la pantalla.

Esto garantiza que ninguna ejecución comience desde la misma configuración.

#### Parámetros constantes

- Número de población
- Cantidad de partículas
- Radio de interacción
- Velocidades máximas
- fricción
- estructura geeneral de la matriz
- Sistema de explosiones
- Interacción mediante el usuario

#### Parámetros variables

- Posición inicial
- Velocidad inicial
- Evolución del ruido Perlin
- Presión emocional
- Momento de las explosiones
- Reorganización espacial
- Intervención del usuario

#### Apariencia

- Las partículas utilizan hilos de tinta con gradientes y variaciones de saturación y brillo.
- La apariencia no comunica directamente el significado conceptual.
- Los cambios visuales reflejan únicamente el estado dinámico interno de cada partícula, reforzando la percepción del comportamiento emergente.

#### Interacción

- El usuario nunca controla directamente las partículas.
- Su intervención modifica únicamente las condiciones del sistema.
- Mover el cursor genera corrientes que alteran las trayectorias.
- Hacer clic produce ondas que reorganizan temporalmente la distribución.
- El sistema continúa evolucionando incluso sin interacción.


### Pruebas Relevantes



https://github.com/user-attachments/assets/6a21e146-53ff-4c92-89ed-0b1ab185e1e9


---

Esta fue una prueba prototipo, inicialmente no se veía bien pero ver las interacciones me dió una idea para crear interacciones más únicas que pudiesen llegar a no permitir que el sistema encuentre un reposo absoluto, no había pensado en la idea de diseño

---

La segunda prueba que considero útil fue orientada al diseño y a ajustar el comportamiento con el ruido de perlin para generar un movimiento más constante y que no lleve a las partículas al equilibrio


https://github.com/user-attachments/assets/08ff1e8c-e48e-4ce3-876d-03e0e2f8d38f

---

Por último esta última prueba fue para añadir la interacción de usuarios y un apartado más estético.

https://github.com/user-attachments/assets/bfa38bd1-4eb3-4366-9654-b5e4a1d4bf45


### Hallazgos

- La contradicción entre espíritu y emociones puede expresarse mediante reglas de interacción y no únicamente mediante representación visual.
- Las relaciones asimétricas producen comportamientos considerablemente más ricos que una matriz completamente simétrica.
- La acumulación de presión genera eventos emergentes más significativos que una atracción constante.
- Una población reguladora numerosa permite que el sistema conserve identidad incluso después de perturbaciones intensas.
- La intervención del usuario resulta más interesante cuando modifica condiciones del sistema y no cuando controla directamente las partículas.


### Autoevaluación

| Criterio                                                                          |     Peso | Valoración |    Aporte |
| --------------------------------------------------------------------------------- | -------: | ---------: | --------: |
| La intención es clara y perceptible en el comportamiento.                         |      20% |    **95%** |  **19.0** |
| Los tipos, cantidades, matriz y parámetros están justificados desde la intención. |      25% |    **95%** | **23.75** |
| Comprendo y puedo modificar el funcionamiento técnico del sistema.                |      20% |   **100%** |  **20.0** |
| El sistema produce variaciones con una identidad reconocible.                     |      15% |    **95%** | **14.25** |
| Experimenté, comparé, seleccioné y descarté con criterios claros.                 |      10% |    **90%** |   **9.0** |
| Puedo distinguir y sustentar lo diseñado y lo emergente.                          |      10% |    **95%** |   **9.5** |
| **Total**                                                                         | **100%** |            |  **95.5** |

# [Link al proyecto](https://editor.p5js.org/KiwisCas/full/FVun9Rg6L)

