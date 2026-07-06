---
marp: true
math: mathjax
paginate: true
style: |
  section { font-size: 27px; line-height: 1.35; }
  section.lead { text-align: center; }
  section.lead h1 { font-size: 2.1em; }
  h2, h3 { color: #12394f; }
  code { font-size: 0.84em; }
  table { font-size: 0.77em; }
  img { max-height: 430px; }
  .columns { display: grid; grid-template-columns: 1fr 1fr; gap: 32px; align-items: start; }
  .callout { background: #eef6fb; border-left: 6px solid #2f6f9f; border-radius: 6px; padding: 0.7em 0.9em; }
  .bridge { background: #f7f8fa; border-left: 6px solid #6b7280; border-radius: 6px; padding: 0.65em 0.9em; }
  .warn { background: #fff4df; border-left: 6px solid #b7791f; border-radius: 6px; padding: 0.65em 0.9em; }
  .example-space { background: #eef8f1; border-left: 6px solid #2f855a; border-radius: 6px; padding: 0.65em 0.9em; }
  .small { font-size: 0.82em; }
---
<!-- _class: lead -->

![w:125](images/utfsm.png)

# IPD434
## Seminario de Soft Computing
### Presentación y mapa del curso

Dr. Nicolás Gálvez Ramírez<br>
Dr. Patricio Olivares Roncagliolo<br>
Universidad Técnica Federico Santa María

---

## La pregunta central

¿Cómo obtener una solución útil cuando el problema es demasiado grande, incierto o difícil de modelar con exactitud?

La pregunta no enfrenta métodos exactos y aproximados como alternativas excluyentes. Primero se debe establecer qué nivel de precisión exige la decisión y qué recursos están disponibles.

<div class="columns">
<div>

**Hard Computing**

- Modelo preciso.
- Datos completos.
- Solución exacta.
- Reglas deterministas.

</div>
<div>

**Soft Computing**

- Incertidumbre o imprecisión.
- Aprendizaje desde datos.
- Búsqueda aproximada.
- Soluciones robustas y viables.

</div>
</div>

<div class="callout">
El curso estudia cuándo una aproximación es razonable, cómo construirla y con qué evidencia evaluarla.
</div>

---

## Resultados de aprendizaje

Al finalizar el curso se espera que cada estudiante pueda:

1. **Analizar** la complejidad de un problema y de su espacio de soluciones.
2. **Distinguir** los criterios de aprendizaje y búsqueda de distintos modelos de Soft Computing.
3. **Seleccionar y aplicar** técnicas de lógica difusa, computación evolutiva o redes neuronales.
4. **Evaluar** una solución mediante métricas, líneas base y supuestos explícitos.
5. **Comunicar** el método y sus resultados en formato de proyecto y artículo breve.

---

## Prerrequisitos y punto de partida

Curso de postgrado del Departamento de Electrónica:

- Magíster en Ciencias de la Ingeniería Electrónica.
- Doctorado en Ingeniería Electrónica.
- Carga académica: **10 SCT**.

Prerrequisitos curriculares:

- ELO320: Estructuras de Datos y Algoritmos.
- MAT043: Procesos Aleatorios y Aplicaciones.

<div class="bridge">
Se asumirá manejo de programación, estructuras de datos, probabilidad y álgebra lineal básica. Los notebooks se usarán para experimentar, no para reemplazar la formulación.
</div>

---

## Mapa conceptual del curso

$$
\text{problema complejo}
\longrightarrow
\begin{cases}
\text{razonar con imprecisión} & \text{lógica difusa}\\
\text{buscar en espacios grandes} & \text{computación evolutiva}\\
\text{aprender desde datos} & \text{redes neuronales}
\end{cases}
\longrightarrow
\text{decisión evaluada}
$$

Las tres familias comparten una idea: sacrificar la exactitud o una estructura rígida cuando ello permite obtener una solución práctica, verificable y suficientemente buena.

“Suficientemente buena” debe traducirse en un criterio medible: error máximo, calidad mínima, tiempo de respuesta o costo computacional aceptable.

---

## Secuencia de unidades

| Unidad | Pregunta que responde |
|---|---|
| 1. Introducción a Soft Computing | ¿Por qué una aproximación puede ser necesaria? |
| 2. Sistemas difusos | ¿Cómo razonar con conceptos graduales? |
| 3. Computación evolutiva | ¿Cómo explorar un espacio de soluciones? |
| 4. Redes neuronales | ¿Cómo aprender una función desde datos? |
| 5. Especialidades | ¿Cómo extender y combinar las técnicas base? |

<div class="callout">
Cada unidad cambia el mecanismo, pero mantiene el mismo ciclo: representación, aprendizaje o búsqueda, evaluación y validación.
</div>

---

## Caso conductor

Supongamos un sistema que debe ajustar recursos de cómputo según carga, latencia y consumo energético.

| Necesidad | Técnica candidata |
|---|---|
| Traducir «carga alta» o «latencia aceptable» | Sistema difuso |
| Optimizar umbrales y políticas | Algoritmo evolutivo |
| Predecir carga futura | Red neuronal |
| Combinar predicción y decisión interpretable | Sistema híbrido |

La selección final dependerá de datos disponibles, costo de evaluación, interpretabilidad y restricciones operacionales.

Este caso muestra que una solución completa puede combinar técnicas: predecir la carga, optimizar una política y expresar la decisión final mediante reglas comprensibles.

---

## Evaluación del curso

$$
NF=0.5\,C+0.3\,PR+0.2\,LP
$$

| Símbolo | Evidencia | Ponderación |
|---|---|---:|
| $C$ | Certamen sobre tópicos base | 50 % |
| $PR$ | Proyecto aplicado y presentación final | 30 % |
| $LP$ | Lectura y presentación de un artículo | 20 % |

Aprobación u homologación:

- Postgrado: $NF\ge 70$.
- Pregrado: $NF\ge 55$.

---

## Certamen: dominio conceptual y aplicado

El certamen evalúa los tres pilares base:

- Lógica y sistemas difusos.
- Computación evolutiva.
- Redes neuronales artificiales.

Una respuesta completa debe incluir:

1. Representación del problema.
2. Mecanismo de inferencia, búsqueda o aprendizaje.
3. Desarrollo o cálculo.
4. Interpretación del resultado.

El certamen evalúa tanto la ejecución del método como la capacidad de explicar por qué corresponde al problema planteado.

---

## Proyecto: resolver y demostrar

El proyecto aborda un problema complejo de telecomunicaciones o ciencias de la computación.

Entregables centrales:

- Formulación del problema y revisión del estado del arte.
- Técnica seleccionada y justificación.
- Protocolo experimental reproducible.
- Resultados comparados con una línea base.
- Artículo breve y presentación final.

<div class="warn">
Un modelo sofisticado sin una pregunta clara, una línea base o métricas pertinentes no constituye evidencia de una mejor solución.
</div>

---

## Lectura y presentación de artículo

La lectura conecta la técnica con investigación publicada en:

- lógica difusa;
- redes neuronales;
- computación evolutiva;
- algoritmos bioinspirados;
- sistemas híbridos.

Guion sugerido:

1. Problema y brecha.
2. Hipótesis o propuesta.
3. Método y datos.
4. Resultado principal.
5. Limitaciones y posibilidad de reproducción.

---

## Criterios mínimos de una solución experimental

- **Pregunta:** qué se intenta explicar, predecir u optimizar.
- **Representación:** variables, codificación y restricciones.
- **Datos o función objetivo:** procedencia y supuestos.
- **Comparación:** línea base y condiciones equivalentes.
- **Métricas:** relacionadas con el objetivo real.
- **Reproducibilidad:** semillas, particiones, versiones y parámetros.
- **Análisis:** incertidumbre, costo y limitaciones.

<div class="callout">
El resultado no es solamente una cifra: debe existir una cadena argumental que permita confiar en ella.
</div>

---

## Herramientas de trabajo

- Presentaciones en Markdown compatible con Marp.
- Fórmulas en LaTeX mediante MathJax.
- Notebooks Jupyter para ejemplos ejecutables.
- Python y las bibliotecas específicas de cada unidad.
- Control de versiones para separar código, datos y resultados.

Los notebooks complementan la clase con experimentos pequeños. El desarrollo conceptual y las conclusiones permanecen en la presentación o el informe.

Para reproducir un resultado se deben registrar, como mínimo, las versiones de las bibliotecas, las semillas aleatorias y los datos utilizados.

---

## Qué debe quedar claro hoy

- Soft Computing reúne técnicas diferentes para problemas difíciles, inciertos o mal definidos.
- El curso exige justificar la elección del método y evaluar su resultado.
- La secuencia avanza desde el problema y su complejidad hacia tres familias de solución.
- El proyecto integra formulación, implementación, experimento y comunicación.

<div class="bridge">
La siguiente unidad estudia el espacio de búsqueda y la complejidad computacional que motivan el uso de aproximaciones.
</div>

---

## Referencias y material base

- Material original de IPD434, notebook `00_Introduccion.ipynb`.
- L. A. Zadeh, “Fuzzy Logic, Neural Networks, and Soft Computing”, *Communications of the ACM*, 1994.
- D. E. Goldberg, *Genetic Algorithms in Search, Optimization, and Machine Learning*, Addison-Wesley, 1989.
- I. Goodfellow, Y. Bengio y A. Courville, *Deep Learning*, MIT Press, 2016.
