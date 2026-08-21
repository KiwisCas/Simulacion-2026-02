# Bitácora de Diseño: Instrumento Visual para *Les Alpx*
**Floating Points — álbum *Crush***
**URL pública:** [https://kiwiscas.github.io/InstrumentoLesAlpx/](https://kiwiscas.github.io/InstrumentoLesAlpx/)

**Bridge Opcional: **[https://github.com/KiwisCas/VisualesBridge](https://github.com/KiwisCas/VisualesBridge)

---

## 1. Introducción y Mapa del Sistema

Esta bitácora documenta el diseño, desarrollo, verificación y conducción de mi instrumento visual generativo para la interpretación en vivo de *LesAlpx* de Floating Points. Tomando como base el repositorio experimental `forces-instrument-u3`, construí un sistema donde la música no activa mecánicamente animaciones (evitando el uso de FFT o análisis de audio directo), sino que actúa como el catalizador de mi escucha para conducir un sistema dinámico de fuerzas en tiempo real.

### Arquitectura técnica (Contrato Común)
*   **Contrato técnico:** Web + Three.js + WebGPURenderer + TSL (Three Shading Language) + GPU Compute + Vite + Publicación web.
*   **Estado (Buffers de GPU):** Gestionado mediante `instancedArray` de Three.js para almacenar `positionBuffer`, `velocityBuffer`, `homePositionBuffer` (posición objetivo dinámica) y un `noiseColorBuffer` auxiliar para la coloración por clústeres.
*   **Fuerzas y Cómputo:** Implementadas mediante Compute Shaders con funciones de cómputo en paralelo (TSL) en el archivo `createSimulation.js`.
*   **Integración:** Procesada en cada frame a través de `renderer.compute()`, aplicando el paso temporal (`dt`), el multiplicador de escala (`timeScale`), la fuerza de atracción elástica y la amortiguación (`damping`).
*   **Renderizado:** Representado por un `InstancedMesh` con `SpriteNodeMaterial` configurado con mezcla aditiva (`AdditiveBlending`) para maximizar la presencia lumínica.
*   **Controles:** Panel centralizado modular (`labPanel.js`) que modifica los `uniforms` compartidos (`parameters.js`) de forma síncrona con el bucle de render (`main.js`).

---

## 2. Ficha de Fuerzas y Comportamiento Emergente

El instrumento opera bajo el principio de que el comportamiento visual debe emerger de las condiciones dinámicas de los campos de fuerza y no de trayectorias pre-animadas.

### 2.1 Fuerza Central de Atracción Elástica (Núcleo)
*   **Ecuación de Integración:**
    $$ \vec{F} = (\vec{p}_{\text{home}} - \vec{p}) \times \text{returnSpeed} $$
    $$ \vec{v}_{t+1} = (\vec{v}_t + \vec{F} \cdot dt) \times \text{damping} $$
    $$ \vec{p}_{t+1} = \vec{p}_t + \vec{v}_{t+1} \cdot dt $$
*   **Descripción direccional:** Las partículas son atraídas incesantemente hacia un buffer objetivo (`homePosition`) que se reescribe según el bioma activo. El parámetro `damping` actúa como una viscosidad del medio, previniendo oscilaciones infinitas.
*   **Parámetros expresivos principales:**
    *   `returnSpeed` (ajustado a 75.0): Define la rigidez y la velocidad de respuesta con la que las partículas convergen hacia sus formas geométricas.
    *   `damping` (0.92): Conservación de inercia tras recibir impulsos cinéticos.
    *   `maxSpeed` (12.0): Techo de velocidad que limita la energía cinética máxima del sistema.

### 2.2 Anillos de Saturno con Rotación de Rodrigues y Viento Armónico
*   **Descripción geométrica y física:** Las partículas se distribuyen en capas concéntricas persistentes (`fixedRingRatio`). La rotación independiente por capa se resuelve aplicando la fórmula de la matriz de rotación de Rodrigues alrededor de un eje $3D$ normalizado y pseudoaleatorio (`axis`). Cuando el viento está activo, se sustituye el cono rígido por un campo de distorsión basado en ondas sinusoidales armónicas entrelazadas:
    $$ \text{wave}_1 = \sin(x_{\text{pos}} \cdot \text{freq} \cdot 0.4 + \text{time} \cdot 2.0) $$
    $$ \text{wave}_2 = \cos(\text{baseAngle} \cdot 3.0 + x_{\text{pos}} \cdot \text{freq} \cdot 0.8) $$
    $$ \text{patternDisplacement} = \text{wave}_1 \cdot \text{wave}_2 \cdot \text{amp} $$

### 2.3 Bioma de Diamante e Inclusión de Tornados Internos (Diegéticos)
*   **Descripción:** Un sistema híbrido donde el 80% de las partículas estructuran una bipirámide u octaedro vertical oscilante (`diamondHeight`, `diamondScale`), mientras que el 20% restante genera vórtices helicoidales miniatura en su núcleo interno, dotando al volumen de turbulencia contenida sin destruir la geometría principal.

---

## 3. Registro de Pruebas

1. **Prueba Base 1 — Inicialización en Esfera:**
   * *Condición:* Ejecución de `reset()` o pulsación de la tecla `R`.
   * *Predicción y Observación:* Nube de partículas condensada uniformemente en un volumen esférico mediante distribución volumétrica con $r^{1/3}$, con velocidad inicial nula. Confirmado.
2. **Prueba Base 2 — Rigidez de Atracción (ReturnSpeed):**
   * *Condición:* Elevar el `returnSpeed` a 75.0 y alternar formaciones.
   * *Predicción y Observación:* Las partículas convergen de forma inmediata a las nuevas aristas del hogar sin estelas difusas prolongadas. Confirmado.
3. **Prueba Base 3 — Confinamiento Espacial (Wrap-Around):**
   * *Condición:* Activar el flujo unidireccional de viento (`windActive`) y mantenerlo durante lapsos prolongados.
   * *Predicción y Observación:* Gracias al operador modular sobre el espacio delimitado (`mod(p + half, bounds) - half`), las partículas cruzan el límite frontal y reaparecen atrás de manera fluida y sin discontinuidades visuales. Confirmado.
4. **Prueba Base 4 — Estabilidad de Asignación en Anillos:**
   * *Condición:* Incrementar dinámicamente las divisiones de anillos (`ringDivisions`) mediante la tecla `D`.
   * *Predicción y Observación:* Al emplear un hash constante por partícula (`fixedRingRatio`), se evita el salto brusco de identidad entre anillos interiores y exteriores, logrando una expansión limpia. Confirmado.
5. **Prueba Base 5 — Respuesta al Impulso de Beat:**
   * *Condición:* Disparar manualmente el evento de *Beat* (`B`) con `beatPower` elevado.
   * *Predicción y Observación:* Expansión instantánea de escala en el material de los sprites y destello cromático blanco de corta duración, disipándose exponencialmente por el factor `beatDecay`. Confirmado.

---

## 4. Score Visual e Interpretación de *LesAlpx*

El instrumento opera como un sistema de conducción humana y no como un visualizador reactivo de audio (FFT). La partitura temporal estructura mis decisiones interpretativas en directo:

| Tramo Temporal | Intención Sonora / Escucha | Gesto de Conducción y Bioma Activo |
| :--- | :--- | :--- |
| **0:00 – 1:00** | Latencia estática, atmósfera abierta y espacial. | **Esfera (R)**. `returnSpeed` bajo (25.0). Estabilidad visual y reposo cinético. |
| **1:00 – 2:20** | Acumulación geométrica gradual, rítmica interna. | **Anillos (D)**. Activación progresiva de divisiones y rotación inicial de capas (`G`). |
| **2:20 – 3:00** | Fricción armónica, tensión y pérdida de orden. | **Viento y Distorsión de Patrones (W)**. Elevación de la amplitud de onda para simular vórtices. |
| **3:00 – 3:30** | Clímax rítmico, ruptura y máxima densidad. | **Diamante / Tornados (M / T)**. Disparos manuales rítmicos de **Beat (B)** para acentuar los golpes de la pieza. |
| **3:30 – Final** | Resolución, orden recuperado y respiración. | **Mándala / Esfera (N / R)**. Reducción paulatina del `timeScale` general para cerrar el ciclo armónico. |

---

## 5. Bitácora de Inteligencia Artificial y Criterio de Diseño

El proceso de desarrollo con IA siguió estrictamente un ciclo de especificación, verificación y modificación localizada de código:

1. **Definición de Asignación en Anillos:**
   * *Problema inicial:* Al cambiar las divisiones de los anillos, el uso de un módulo dependiente generaba saltos abruptos de partículas entre capas.
   * *Propuesta de la IA:* Utilizar un factor fijo basado en hash por índice (`fixedRingRatio`).
   * *Criterio de Aceptación:* Acepté la solución porque garantizó la estabilidad estructural y una expansión orgánica sin alterar la lógica de los compute shaders en TSL.
2. **Evolución del Viento y Geometría:**
   * *Propuesta inicial de la IA:* Un cono rígido orientado hacia la cámara.
   * *Decisión de Rechazo y Corrección:* Rechacé el cono por limitar la libertad expresiva y parecer una animación fija. Le exigí a la IA implementar un campo de distorsión armónica con ondas sinusoidales dependientes de la posición $X$ y el tiempo, logrando una turbulencia viva.
3. **Optimización de Interfaz en Vivo:**
   * *Ajuste:* Separación de los controles generales de laboratorio (`labPanel.js`) frente a las acciones rápidas de escenario (`performancePanel.js`), asegurando una manipulación limpia durante la interpretación.

---

## 6. Autoevaluación Ponderada

| Criterio | Peso | Valoración | Justificación y Evidencia Concreta |
| :--- | :--- | :--- | :--- |
| **Trazabilidad y comprensión del sistema** | 25% | **94** | Identificación absoluta de la arquitectura basada en WebGPU/TSL, separación de buffers en GPU (`homePosition` vs `position`) y distribución de responsabilidades en los archivos del proyecto. |
| **Verificación del algoritmo de fuerzas** | 25% | **93** | Aislamiento y comprobación de las ecuaciones de atracción elástica, calibración del amortiguamiento (`damping`) y validación del comportamiento modular del viento. |
| **Diseño de fuerzas e intención** | 20% | **95** | El comportamiento visual emerge rigurosamente de la física de partículas y campos dinámicos, eliminando por completo trayectorias fijas o animaciones interpoladas. |
| **Instrumento, score e interpretación** | 15% | **92** | Coherencia total entre la escucha de *LesAlpx*, la estructuración del score temporal y la conducción manual mediante pocos parámetros expresivos sin intervención de FFT. |
| **Experimentación y criterio frente a la IA** | 10% | **96** | Uso crítico de la IA: rechazo de propuestas estáticas (cono rígido), refactorización de algoritmos de distribución en anillos y supervisión matemática del código generado. |
| **Entrega técnica y documentación** | 5% | **100** | Proyecto publicado y funcional en la URL pública, código modular limpio y bitácora detallada conforme a los requerimientos. |

### Cálculo de Puntuación Final y Nota
* **Trazabilidad:** $25 \times 0.94 = 23.5$
* **Verificación:** $25 \times 0.93 = 23.25$
* **Diseño e intención:** $20 \times 0.95 = 19.0$
* **Instrumento y Score:** $15 \times 0.92 = 13.8$
* **Criterio frente a IA:** $10 \times 0.96 = 9.6$
* **Entrega técnica:** $5 \times 1.0 = 5.0$
* **Total Puntos Ponderados:** **94.15 / 100**

**Nota Equivalente (Escala de 0.0 a 5.0):** **4.7 / 5.0**

