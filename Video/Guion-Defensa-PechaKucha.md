# Guion de Defensa Técnica en Video — PechaKucha (TP 1)

> **Materia:** Ciencia de Datos / Inteligencia Artificial — UTN FRSF — 2026, 1er Cuatrimestre
> **Formato:** PechaKucha original → **20 diapositivas × 20 segundos = 6 min 40 s exactos**
> **Equipo (4 integrantes):** Gaitán · Debona · Agüero · Ocampo
> **Fecha límite:** 25/06/2026

---

## Verificación de cumplimiento de la consigna

| Requerimiento de la consigna | Cómo se cumple en este guion |
|---|---|
| PechaKucha: 20 diapositivas × 20 s = **6:40 exactos** | 20 slides con narración calibrada a 20 s c/u (ver tiempos en cada slide) |
| Extensión del TP original **incorporando un MLP** | Parte 1 y 2 desarrollan el diseño y entrenamiento del MLP |
| **Todos los integrantes** visibles y audibles a cámara | El guion se divide en **4 partes = 4 integrantes**, 5 slides cada uno |
| **Eje 1** — Diseño/entrenamiento del MLP + justificación de arquitectura (capas, neuronas, activaciones) | Parte 1 (slides 4–5) y Parte 2 completa (slides 6–10) |
| **Eje 2** — Impacto de normalización/estandarización **para la red**, comparado con modelos de la Parte 2 | Parte 3 (slides 11–13) |
| **Eje 3** — Comparación de métricas MLP vs **Base y Ensamblado** + discusión técnica | Parte 3 (slides 14–15) y Parte 4 (slides 16–17) |
| **Eje 4** — Conclusión: elección de modelo para planta real, precisión vs interpretabilidad | Parte 4 (slides 18–20) |
| Enfoque en **justificar decisiones**, sin releer el informe | Toda la narración responde "por qué", no describe pasos |

---

## Cómo usar este guion

- El guion está dividido en **4 PARTES, una por integrante**. Cada integrante graba **5 diapositivas seguidas** mirando a cámara. Así se garantiza que los cuatro aparezcan hablando.
- Cada diapositiva dura **20 s exactos**. La narración está calibrada a ~45–55 palabras (ritmo ≈ 150 ppm). **No agregar texto**: si sobra tiempo, una breve pausa es mejor que apurarse.
- Configurar el avance **automático** cada 20 s (PowerPoint: *Transiciones → Avanzar después de 00:20*). Ensayar con cronómetro al menos 2 veces.
- **No leer el informe.** El foco es **justificar decisiones** (por qué esta arquitectura, por qué estandarizar, por qué este modelo en planta).

### Reparto general

| Parte | Integrante | Diapositivas | Tiempo | Contenido / Ejes |
|---|---|---|---|---|
| **Parte 1** | Integrante 1 | 1 – 5 | 0:00 – 1:40 | Contexto + inicio del Eje 1 (motivación y diseño del MLP) |
| **Parte 2** | Integrante 2 | 6 – 10 | 1:40 – 3:20 | Eje 1 completo (arquitectura, hiperparámetros, entrenamiento) |
| **Parte 3** | Integrante 3 | 11 – 15 | 3:20 – 5:00 | Eje 2 + inicio del Eje 3 (preprocesamiento y métricas) |
| **Parte 4** | Integrante 4 | 16 – 20 | 5:00 – 6:40 | Eje 3 (cierre) + Eje 4 (comparación y conclusión) |

> *Las diapositivas NO llevan nombres propios: el pie de página muestra solo «Integrante 1/2/3/4», así que el equipo asigna libremente quién toma cada bloque de 5 slides. Los nombres reales solo aparecen en la portada (saludo) y en el cierre (agradecimiento). Lo esencial es respetar las 5 slides por integrante.*

---

# 🟦 PARTE 1 — Integrante 1: (slides 1–5 · 0:00 – 1:40)
### *Contexto del problema e inicio del Eje 1 (motivación y diseño del MLP)*

## Diapositiva 1 — Portada y problema (0:00 – 0:20)
*Visual: título del TP, foto del equipo o logos, imagen de máquina industrial.*

> Buenas. Somos Gaitán, Debona, Agüero y Ocampo. Extendemos nuestro Trabajo Práctico de mantenimiento predictivo industrial: dado un conjunto de variables de operación de una máquina, queremos predecir si va a **fallar**. Hoy sumamos una red neuronal —un Perceptrón Multicapa— y la comparamos contra los modelos clásicos que ya entrenamos.

## Diapositiva 2 — El dato y el target (0:20 – 0:40)
*Visual: tabla con las 8 features y la torta del target balanceado (~51,5% falla).*

> Trabajamos con **8 variables de entrada**: cinco continuas —temperatura de aire y de proceso, velocidad, torque y desgaste de herramienta— y el tipo de producto codificado en tres columnas. El target es binario: falla o no falla. Las clases quedaron prácticamente balanceadas, así que no aplicamos técnicas de balanceo.

## Diapositiva 3 — ¿Qué heredamos de Ciencia de Datos? (0:40 – 1:00)
*Visual: pipeline EDA → limpieza → One-Hot → estandarización → train/test.*

> Reutilizamos el mismo preprocesamiento del TP original: eliminamos outliers, imputamos faltantes, codificamos `product_type` con **One-Hot** —porque demostramos que es nominal, no ordinal— y estandarizamos con Z-score. Usar exactamente los mismos datos para todos los modelos es lo que hace **justa** la comparación que veremos.

## Diapositiva 4 — ¿Por qué un MLP? (1:00 – 1:20) · *Eje 1*
*Visual: esquema simple de red feedforward.*

> ¿Por qué un Perceptrón Multicapa? Porque, a diferencia de los modelos lineales, **aprende relaciones no lineales** entre variables, y en el EDA vimos interacciones complejas: el torque bajo dispara la falla, la velocidad alta también, y el efecto depende del tipo de producto. El MLP puede capturar esas combinaciones sin que se las indiquemos.

## Diapositiva 5 — Filosofía de diseño (1:20 – 1:40) · *Eje 1*
*Visual: diagrama "embudo" 8 → capas decrecientes → 1.*

> Diseñamos con una idea clave: **simplicidad acorde al problema**. Con solo 8 features, una red muy profunda solo agregaría sobreajuste. Optamos por una arquitectura en **embudo**: la red comprime progresivamente la información, quedándose con los patrones que anticipan la falla. Lo concreto lo explica mi compañero.

---

# 🟩 PARTE 2 — Integrante 2: (slides 6–10 · 1:40 – 3:20)
### *Eje 1 completo: arquitectura, hiperparámetros y entrenamiento del MLP*

## Diapositiva 6 — Arquitectura final (1:40 – 2:00) · *Eje 1*
*Visual: 8 entrada → 32 (ReLU) → 32 (ReLU) → 1 (Sigmoid).*

> La arquitectura: capa de entrada de **8 neuronas**, una por feature; **dos capas ocultas** de 32 neuronas; y una **única neurona de salida**. Dos capas alcanzan para 8 variables; una sola salida basta porque es clasificación binaria. En total: apenas **1.377 parámetros**, un modelo muy liviano.

## Diapositiva 7 — Funciones de activación (2:00 – 2:20) · *Eje 1*
*Visual: curvas ReLU y Sigmoid lado a lado.*

> En las capas ocultas usamos **ReLU**: es el estándar moderno, evita el desvanecimiento del gradiente y es muy eficiente. En la salida usamos **Sigmoid**, que mapea el resultado al rango cero–uno, interpretándolo directamente como **probabilidad de falla**. No usamos Softmax porque eso aplica cuando hay varias clases de salida.

## Diapositiva 8 — Función de pérdida y regularización (2:20 – 2:40) · *Eje 1*
*Visual: fórmula binary_crossentropy + esquema de Dropout.*

> Como pérdida elegimos **binary cross-entropy**, la función canónica para clasificación binaria: penaliza fuerte cuando el modelo está seguro y se equivoca. Para evitar sobreajuste contemplamos **Dropout** y **regularización L2**, dejando que la búsqueda automática decida cuánta regularización realmente hacía falta.

## Diapositiva 9 — Búsqueda de hiperparámetros (2:40 – 3:00) · *Eje 1*
*Visual: tabla del espacio de búsqueda + logo Keras Tuner.*

> No elegimos los hiperparámetros a mano. Usamos **Keras Tuner con RandomSearch**: 50 ensayos sobre un espacio de casi mil combinaciones —neuronas, learning rate, dropout, L2 y optimizador—. Optimizamos sobre **val_loss**, no accuracy, porque en fallas industriales nos importa **estimar bien la probabilidad**, no solo acertar la etiqueta.

## Diapositiva 10 — Configuración óptima y entrenamiento (3:00 – 3:20) · *Eje 1*
*Visual: config final (Adam, lr 0.01, batch 64) + curvas de loss/accuracy.*

> El mejor modelo: optimizador **Adam**, learning rate 0.01, batch size 64, sin dropout ni L2 —la red era tan chica que no los necesitó—. Entrenamos con **EarlyStopping** para frenar al dejar de mejorar. Las curvas convergen bien, con una validación algo inestable, propia de un dataset acotado.

---

# 🟨 PARTE 3 — Integrante 3: (slides 11–15 · 3:20 – 5:00)
### *Eje 2 (sensibilidad al preprocesamiento) + inicio del Eje 3 (métricas del MLP)*

## Diapositiva 11 — ¿Por qué la red es sensible a la escala? (3:20 – 3:40) · *Eje 2*
*Visual: rangos crudos: RPM ~1200–1700 vs temp ~296–303.*

> Acá está el punto central del eje 2. Las variables crudas tienen escalas dispares: las **RPM rondan los miles**, las temperaturas apenas decenas. Una red neuronal aprende por **descenso de gradiente**, que es muy sensible a la escala: sin tratamiento, las RPM dominarían el aprendizaje y aplastarían a las demás variables.

## Diapositiva 12 — El impacto de estandarizar en la red (3:40 – 4:00) · *Eje 2*
*Visual: fórmula Z-score + convergencia rápida vs lenta.*

> Por eso aplicamos **estandarización Z-score**: media cero, desvío uno. El efecto en la red es directo: el gradiente se mueve en un terreno parejo, **converge más rápido y estable**, y todas las variables compiten en igualdad. Para el MLP esto **no es opcional**: es condición para que entrene bien.

## Diapositiva 13 — Comparación con los modelos de la Parte 2 (4:00 – 4:20) · *Eje 2*
*Visual: tabla "sensible a escala": MLP y KNN ✔ / Árboles ✘.*

> Comparado con la Parte 2, el contraste es claro. El **MLP y KNN dependen** de la escala. En cambio los **árboles —Decision Tree y Random Forest— son invariantes**: parten por umbrales, no por distancias, así que estandarizar no cambia sus resultados. Por eso el preprocesamiento es **crítico para la red** y casi indiferente para el ensamblado.

## Diapositiva 14 — Resultados del MLP (4:20 – 4:40) · *Eje 3*
*Visual: matriz de confusión + métricas del MLP.*

> El MLP rinde muy bien: **96% de accuracy**, F1 de 0.96 y un **AUC de 0.991**. Ajustamos el umbral de decisión a **0.3** en lugar de 0.5, priorizando el **recall**: alcanzamos **98% de fallas detectadas**. En mantenimiento, dejar pasar una falla real es mucho más caro que una falsa alarma.

## Diapositiva 15 — Tabla comparativa de métricas (4:40 – 5:00) · *Eje 3*
*Visual: tabla MLP vs KNN vs Árbol vs Naive Bayes vs Random Forest.*

> Puestos en una tabla: Naive Bayes queda atrás con 0.84. KNN y Árbol —los modelos **base**— rondan 0.95–0.96. El **MLP llega a 0.96 con AUC 0.991**. Y el **ensamblado, Random Forest, lidera con 0.972 y AUC 0.997**. El detalle clave que retoma mi compañero: la red, pese a su complejidad, **no supera al ensamblado**.

---

# 🟥 PARTE 4 — Integrante 4: (slides 16–20 · 5:00 – 6:40)
### *Eje 3 (discusión técnica) + Eje 4 (conclusión y elección del modelo)*

## Diapositiva 16 — MLP frente al ensamblado (5:00 – 5:20) · *Eje 3*
*Visual: barras AUC: Random Forest 0.997 vs MLP 0.991.*

> Comparemos de frente el MLP con el mejor clásico. **Random Forest gana en todas las métricas** —accuracy, F1 y AUC—, aunque por un margen estrecho. Es un resultado revelador: el modelo "más fácil de explicar" —un conjunto de árboles— **iguala o supera** a la red neuronal en este problema.

## Diapositiva 17 — ¿Se justifica la complejidad del MLP? (5:20 – 5:40) · *Eje 3*
*Visual: balanza complejidad vs ganancia.*

> Entonces, la pregunta de la consigna: ¿la mayor complejidad del MLP se justifica? **Para este problema, no.** Exige estandarización obligatoria, ajuste de muchos hiperparámetros y un entrenamiento más delicado, y aun así **no mejora** al ensamblado. La complejidad solo se paga cuando entrega una ventaja clara, y aquí no la hay.

## Diapositiva 18 — Precisión vs interpretabilidad (5:40 – 6:00) · *Eje 4*
*Visual: eje "caja negra" (MLP) ↔ "explicable" (árbol/RF).*

> El otro eje de decisión es la **interpretabilidad**. El MLP es una **caja negra**: predice bien, pero no explica por qué. Un árbol o un Random Forest dan **reglas e importancia de cada variable**. En una planta, un técnico necesita saber *qué* condición disparó la alarma para actuar, no solo recibir un número.

## Diapositiva 19 — Modelo elegido para la planta (6:00 – 6:20) · *Eje 4*
*Visual: "Modelo recomendado: Random Forest" con sus métricas.*

> Por eso, para una implementación **real en planta**, elegimos **Random Forest**: mejor performance, robustez e **importancia de variables** que respalda cada predicción. El costo de reentrenar, 3,7 segundos, es irrelevante frente a esa ventaja. La red queda como alternativa válida, pero no como la opción óptima aquí.

## Diapositiva 20 — Conclusión y cierre (6:20 – 6:40) · *Eje 4*
*Visual: 3 ideas finales + nombres del equipo.*

> En síntesis: el MLP es potente y exige un preprocesamiento cuidadoso, pero **más complejidad no garantizó mejores resultados**. El criterio ganó: elegimos el modelo que combina **precisión e interpretabilidad** para un entorno industrial real. Gracias —Gaitán, Debona, Agüero y Ocampo.

---

## Anexo — Datos de respaldo (para preguntas / consulta rápida)

**MLP (Perceptrón Multicapa)**
- Arquitectura: 8 → 32 (ReLU) → 32 (ReLU) → 1 (Sigmoid) · 1.377 parámetros · entrenado en CPU
- Optimizador Adam (lr 0.01), batch 64, pérdida `binary_crossentropy`, EarlyStopping, sin Dropout/L2
- Búsqueda: Keras Tuner — RandomSearch, 50 trials, objetivo `val_loss`, umbral de decisión 0.3
- Métricas (test): Accuracy 0.96 · Precision (falla) 0.94 · **Recall (falla) 0.98** · F1 0.96 · **AUC 0.991**

**Modelos clásicos (Base) y Ensamblado (misma partición test)**

| Modelo | Tipo | Accuracy | Precision | Recall | F1 | AUC | Entren. |
|---|---|---|---|---|---|---|---|
| **Random Forest** | Ensamblado | **0.9721** | 0.9658 | 0.9806 | **0.9732** | **0.9970** | 3,74 s |
| MLP | Red neuronal | 0.96 | 0.94 | 0.98 | 0.96 | 0.9911 | — (CPU) |
| KNN | Base | 0.9594 | 0.9389 | 0.9853 | 0.9615 | 0.9586 | 0,01 s |
| Decision Tree | Base | 0.9525 | 0.9462 | 0.9626 | 0.9543 | 0.9587 | 0,04 s |
| Naive Bayes | Base | 0.8437 | 0.8337 | 0.8704 | 0.8516 | 0.9155 | 0,004 s |

**Mensajes clave de la defensa**
1. El MLP necesita estandarización **obligatoria**; los árboles son invariantes a la escala (Eje 2).
2. La red rinde excelente (AUC 0.991) pero **no supera** al ensamblado (AUC 0.997) (Eje 3).
3. La complejidad del MLP **no se justifica** para este problema acotado de 8 features (Eje 3).
4. Decisión final de planta: **Random Forest**, por equilibrio entre precisión e **interpretabilidad** (Eje 4).

---

## Checklist de grabación
- [ ] 20 diapositivas con avance automático cada 20 s.
- [ ] Duración total verificada = **6:40 exactos**.
- [ ] Los **4 integrantes** aparecen y se los oye hablando a cámara (5 slides cada uno).
- [ ] Ensayo cronometrado completo (mínimo 2 pasadas).
- [ ] Audio sincronizado con cada transición de slide.
- [ ] Foco en justificación de decisiones, sin leer el informe.
- [ ] Entrega antes del **25/06/2026**.
