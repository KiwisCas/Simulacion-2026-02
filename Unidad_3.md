# Bitácora de Diseño: Instrumento Visual "LesAlpx"

## 1. Introducción
Esta bitácora documenta el desarrollo de mi instrumento visual generativo, diseñado para interpretar la pieza *LesAlpx* de Floating Points. Mi enfoque ha sido construir un sistema basado en fuerzas dinámicas, donde la música actúa como el catalizador de mi percepción, no como un controlador automatizado.

---

## 2. Mapa del Sistema
El sistema se estructura sobre el proyecto base `forces-instrument-u3` (Web, Three.js, WebGPURenderer, TSL, GPU Compute).

*   **Estado:** Almacenado en buffers instanciados (`positionBuffer`, `velocityBuffer`, `homePositionBuffer`).
*   **Fuerzas:** Implementadas mediante `Fn` en `createSimulation.js`.
*   **Integración:** El `renderer` calcula los *compute shaders* antes de cada frame de renderizado.
*   **Render:** `InstancedMesh` con `SpriteNodeMaterial`.
*   **Controles:** Panel centralizado (configurado en `main.js` y `labPanel.js`) que modifica los `uniforms` en `parameters.js`.

---

## 3. Ficha de Fuerzas: Comportamiento Emergente
He diseñado el comportamiento a partir de dos fuerzas centrales:

### Fuerza A: Atracción Radial (El "Hogar")
*   **Descripción:** Una fuerza elástica que intenta devolver cada partícula a una posición de reposo definida por la geometría seleccionada (esfera, anillo, etc.).
*   **Parámetros:** `returnSpeed`, `damping`.
*   **Predicción:** A valores bajos de `returnSpeed`, las partículas tardan en reordenarse, creando una "estela" de movimiento; a valores altos, la geometría es rígida.
*   **Decisión:** Utilizo valores dinámicos para pasar de la "dispersión" (caos) a la "condensación" (orden) según el score.

### Fuerza B: Distorsión de Campo (Patrón de Viento)
*   **Descripción:** Un campo de ruido senoidal que desplaza las partículas perpendicularmente a su eje principal, alterado por `time`.
*   **Parámetros:** `distortionFrequency`, `distortionAmplitude`.
*   **Predicción:** Ondulaciones tipo líquido que atraviesan la estructura.
*   **Decisión:** Esta es mi herramienta de "tensión". La activo durante los clímax de *LesAlpx* para romper la estructura estática.

---

## 4. Score Visual e Interpretación
He segmentado *LesAlpx* en tramos según mi escucha:

| Tramo (min) | Intención | Acción |
| :--- | :--- | :--- |
| 0:00 - 1:30 | Introducción / Estabilidad | Mantener **Esfera** (baja velocidad). |
| 1:30 - 3:00 | Acumulación / Tensión | Dividir anillos (`handleDivideRing`), subir `rotationSpeed`. |
| 3:00 - 5:00 | Ruptura / Dispersión | Activar **Viento** (`handleToggleWind`), elevar amplitud de distorsión. |
| 5:00 - Final | Reorganización | Condensar todo (`handleSetSphere`), pulso con `triggerBeat`. |

---

## 5. Bitácora de IA
*   **Prompt inicial:** "Modifica `createSimulation.js` para añadir una fuerza de rotación que afecte solo a un subconjunto de anillos en la formación de Saturno."
*   **Aceptación:** Acepté la lógica de `mix(posBase, finalPos, isRotating)` porque permitía transiciones suaves entre estático y rotativo.
*   **Corrección:** La IA propuso inicialmente controlar la posición absoluta con el ratón. **Rechacé esto** porque violaba la restricción de diseño de "no controlar directamente las posiciones".
*   **Ajuste:** Solicité cambiar la lógica a una fuerza rotacional basada en ángulos, manteniendo el sistema dinámico.

---

## 6. Autoevaluación Ponderada

| Criterio | Peso | Valoración | Evidencia/Nota |
| :--- | :--- | :--- | :--- |
| Trazabilidad y comprensión | 25 | 90 | Arquitectura explicada en sección 2. |
| Verificación del algoritmo | 25 | 85 | Identifiqué que `returnSpeed` controla la rigidez. |
| Diseño de fuerzas e intención | 20 | 95 | El score refleja una intención clara. |
| Instrumento, score e interpretación | 15 | 90 | Conducción en tiempo real sin FFT. |
| Experimentación con la IA | 10 | 95 | Registro de descartes en bitácora IA. |
| Entrega técnica | 5 | 100 | URL pública funcional. |
| **TOTAL** | **100** | **91** | |

---
*Este documento fue construido mediante un proceso iterativo de diseño, verificación y modificación en el marco del encargo de diseño para LesAlpx.*
