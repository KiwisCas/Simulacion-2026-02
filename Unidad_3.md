# Bitácora de Diseño: Instrumento Visual "LesAlpx"

**URL pública:** https://kiwiscas.github.io/InstrumentoLesAlpx/

---

## 1. Instrumento Funcional y Mapa del Sistema

El sistema se ha construido expandiendo el proyecto base `forces-instrument-u3` mediante WebGPU, Three.js y TSL[cite: 2]. 

*   **Estado (Buffers):** Las posiciones, velocidades y posiciones objetivo (`homePosition`) se almacenan en instancias de `instancedArray` en la GPU[cite: 3]. Añadí un `noiseColorBuffer` para manejar interpolaciones de color avanzadas[cite: 3].
*   **Fuerzas Centrales:** Todo el movimiento se rige por un *compute shader* (`updateParticles`) que calcula la atracción elástica hacia la posición objetivo, amortiguada por el parámetro `damping`[cite: 3, 4].
*   **Integración:** Ocurre multiplicando la fuerza resultante por `dt` y el factor `timeScale` en cada frame antes de actualizar la posición[cite: 3, 4].
*   **Render:** Se dibuja un `InstancedMesh` con `SpriteNodeMaterial` configurado en `AdditiveBlending` para un efecto lumínico reactivo[cite: 3].
*   **Controles y Arquitectura Extendida (WebSockets):** Inicialmente, el proyecto contaba con paneles integrados en el DOM[cite: 3]. Sin embargo, para cumplir con el requisito de una verdadera "interpretación en vivo" sin elementos visuales distractores en pantalla[cite: 2], migré la capa de control a una interfaz separada comunicada por **protocolos WebSocket**. Esto me permite operar los biomas desde un dispositivo secundario.

---

## 2. Ficha de Fuerzas

Mi comportamiento emergente no depende de trayectorias animadas fijas, sino de una física subyacente[cite: 2, 4].

**Ecuación de Integración Central:**
$ \vec{F} = (\vec{p}_{\text{home}} - \vec{p}) \times \text{returnSpeed} $[cite: 4]
$ \vec{v}_{t+1} = (\vec{v}_t + \vec{F} \cdot dt) \times \text{damping} $[cite: 4]

*   **Direccionalidad:** Las partículas buscan incesantemente su `homePosition`. Dependiendo de la forma activa (Diamante, Tornados, Mándala), el shader reescribe la posición objetivo instantáneamente[cite: 3].
*   **Parámetros expresivos:**
    *   `returnSpeed`: Define la rigidez geométrica. A valores bajos, la transición es viscosa y caótica; a valores altos, es ordenada y repentina[cite: 4].
    *   `damping`: Define cuánta inercia conservan las partículas.
    *   `beatPower`: Multiplicador físico manual que inyecta un empuje posicional transversal y una expansión de escala, rompiendo la inercia rítmicamente sin depender de análisis automático[cite: 2, 3].

---

## 3. Registro de Pruebas

1.  **Prueba Base 1 (Estado Inicial):** Aseguré que las partículas iniciaran estáticas formando una esfera volumétrica en el origen, usando `r1.pow(0.333)`, bloqueando la cámara en el eje X positivo[cite: 4].
2.  **Prueba Base 2 (Transición a Anillos):** Pulsando 'D', verifiqué que el sistema calcula subdivisiones concéntricas de un disco orientado en el plano Y-Z[cite: 4].
3.  **Prueba Base 3 (Límites Espaciales):** Activé el viento continuo (`windActive`). Comprobé que la función `mod` reubica transparentemente las partículas que escapan del `boundsSize`, creando una ilusión de flujo continuo[cite: 3, 4].
4.  **Prueba Base 4 (Vórtices de Tornado):** Aislé la formación de tornados frontales. Verifiqué que el radio cónico creciera en función de la altura, generando espirales que absorben el centro geométrico[cite: 3].
5.  **Prueba Base 5 (Beat Físico):** Disparando el evento de *Beat*, documenté cómo el material reacciona multiplicando la escala por `beatImpulse * beatPower` y forzando un destello lumínico absoluto[cite: 3].
6.  **Prueba Específica (Latencia de WebSockets):** Para validar mi arquitectura cliente-servidor externa, probé disparar los acentos desde mi móvil conectado a la misma red. Confirmé que la latencia era casi nula, permitiendo la interpretación rítmica precisa.

---

## 4. Score Visual e Interpretación de *LesAlpx*

El visualizador es dirigido puramente por intención humana, no por reactividad automática de audio[cite: 2].

| Tiempo Aprox. | Intención Sonora (Escucha) | Conducción y Bioma (Gesto) |
| :--- | :--- | :--- |
| **0:00 - 1:00** | Vacío espacial, sintetizadores iniciales. | **Esfera (R)**. Partículas estáticas[cite: 4]. `returnSpeed` bajo (25.0). |
| **1:00 - 2:20** | Acumulación matemática sutil. | **Anillos (D)**[cite: 4]. Rotación lenta (`rotationSpeed` = 1.5)[cite: 3]. |
| **2:20 - 3:00** | Tensión creciente y fricción. | **Viento Cónico (W)**[cite: 3]. Deformación y perturbación en el eje X[cite: 3]. |
| **3:00 - 3:30** | Ruptura, agresividad (Clímax rítmico). | **Diamante y Tornados (M / T)**[cite: 3]. Ataques secos manuales de **Beat (B)**[cite: 3]. |
| **3:30 - Final** | Resolución, orden, calma armónica. | **Mándala LED (N)**[cite: 3]. Disminución del `timeScale` y reducción de brillo[cite: 3]. |

---

## 5. Bitácora de Inteligencia Artificial

*   **Fase de Limpieza:** Pedí a la IA que eliminara fuerzas irrelevantes de la plantilla original para concentrar todo en el sistema de transición elástica y bloquear la cámara inicial[cite: 4].
*   **Generación de Biomas:** Solicité construir transformaciones de destino (`homePosition`) paramétricas, como el Diamante Oscilante y los Tornados[cite: 3]. La IA implementó exitosamente rotaciones usando fórmulas de Rodrigues[cite: 3].
*   **Decisión de Descarte y Modificación Arquitectónica:** Durante la exploración de la interfaz, la IA me generó un panel lateral integrado `createPerformancePanel` y manipulaciones del DOM[cite: 3]. **Decidí descartar su uso en la presentación final.** Evalué que tener un panel en la misma ventana restaba carácter a la presentación[cite: 2]. Esta limitación me llevó a integrar *WebSockets* (módulo externo propio) para enviar la información desde otra pantalla y justificar la pureza visual de mi propuesta.

---

## 6. Autoevaluación Ponderada

| Criterio | Peso | Valoración | Evidencia y Justificación |
| :--- | :--- | :--- | :--- |
| **Trazabilidad y comprensión** | 25 | **23.75** | Señalo y entiendo la división de buffers GPU (`position` vs `homePosition`) y cómo los `compute shaders` operan en paralelo[cite: 3, 4]. |
| **Verificación del algoritmo** | 25 | **23.75** | Probé el cambio de variables críticas (como el comportamiento al llegar a límites de coordenadas usando `mod`) y documenté sus resultados[cite: 3, 4]. |
| **Diseño de fuerzas e intención** | 20 | **20** | Las formas emergentes se derivan estrictamente de la fórmula de amortiguación[cite: 4] interactuando con los distintos `homePositionBuffer`[cite: 3]. |
| **Instrumento, score e interpretación** | 15 | **14.25** | La relación entre el "Score Visual" temporal y las teclas asignadas para controlar el caos y el orden rítmico están completamente descritas y evitan el uso de FFT[cite: 2, 3]. |
| **Experimentación y criterio IA** | 10 | **10** | Documento mi rechazo a la integración local del panel de control de la IA[cite: 3], proponiendo la estructura cliente-servidor mediante WebSocket para salvar la "experiencia en vivo". |
| **Entrega técnica y doc.** | 5 | **5** | El repositorio se encuentra funcional y la arquitectura cumple con el stack técnico solicitado[cite: 2]. |
| **Total Ponderado** | **100** | **96.75 / 100** | **Nota Equivalente: 4.8 / 5.0** |
