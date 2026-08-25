---
marp: true
math: mathjax
paginate: true
style: |
  section { font-size: 26px; line-height: 1.32; }
  section.lead { text-align: center; }
  section.lead h1 { font-size: 2.05em; }
  h2, h3 { color: #12394f; }
  code { font-size: 0.84em; }
  table { font-size: 0.74em; }
  img { max-height: 410px; }
  section p:has(> img) { text-align: center; }
  .columns { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; align-items: start; }
  .callout { background: #eef6fb; border-left: 6px solid #2f6f9f; border-radius: 6px; padding: 0.65em 0.85em; }
  .bridge { background: #f7f8fa; border-left: 6px solid #6b7280; border-radius: 6px; padding: 0.6em 0.85em; }
  .warn { background: #fff4df; border-left: 6px solid #b7791f; border-radius: 6px; padding: 0.6em 0.85em; }
  .example-space { background: #eef8f1; border-left: 6px solid #2f855a; border-radius: 6px; padding: 0.6em 0.85em; }
  .small { font-size: 0.82em; }
---

<!-- _class: lead -->

![w:125](images/utfsm.png)

# IPD434 · Seminario de Soft Computing
## Computación evolutiva
### De la formulación de un problema a la evolución de soluciones

Dr. Patricio Olivares Roncagliolo<br>
Material basado y ampliado a partir del material desarrollado por el **Prof. Nicolás Gálvez Ramírez**.

---

## El punto de partida: elegir una configuración

Un horario académico contiene muchas formas de asignar cursos, docentes, salas y bloques. Ninguna regla simple entrega de inmediato la mejor combinación.

Problema de búsqueda:

> ¿Cómo encontrar una buena configuración cuando podemos comprobar su calidad, pero no sabemos construir directamente la mejor?

La computación evolutiva responde explorando y transformando una **población** de configuraciones candidatas.

---

## Calidad, factibilidad y espacio de búsqueda

En el horario se distinguen tres elementos:

| Elemento | Pregunta | Ejemplo |
|---|---|---|
| **Candidatos** | ¿Qué configuraciones se pueden proponer? | Asignaciones de cursos, salas y bloques |
| **Calidad** | ¿Qué preferimos entre dos candidatos? | Menos topes, mejor uso de salas, más preferencias atendidas |
| **Condiciones obligatorias** | ¿Qué no se puede violar? | Capacidad, disponibilidad y compatibilidad |

<div class="bridge">
Cuando la incógnita es una configuración y podemos evaluarla, el problema se transforma en uno de <strong>búsqueda y optimización</strong>.
</div>

---

## Ruta conceptual

### Formulación → espacio de búsqueda → metaheurística → población → evidencia

La formulación define la decisión y su evaluación. Luego se selecciona una estrategia para recorrer el espacio de posibilidades.

Cada etapa condiciona la siguiente: una mala formulación o representación no se arregla agregando más generaciones al algoritmo.

---

## ¿Qué parte del problema es la incógnita?

Un mismo sistema puede describirse mediante **entrada**, **modelo** y **salida**. El tipo de problema depende de cuál de esas piezas es la incógnita.

![w:610](images/blackbox.png)

<div class="bridge">
En el horario, el modelo y la evaluación están definidos. La <strong>entrada</strong> es la incógnita: una configuración de decisiones. Esa es la situación de optimización.
</div>

---

## Tres preguntas distintas sobre una misma caja negra

| Problema | Lo que conocemos | Lo que buscamos | Ejemplo |
|---|---|---|---|
| **Simulación** | Una entrada y un modelo | La salida que produce el modelo | Pronóstico meteorológico |
| **Modelado** | Entradas y salidas observadas | Un modelo que explique y generalice | Clasificador de imágenes |
| **Optimización** | Un modelo y cómo evaluar su salida | La entrada que da el mejor resultado | Planificación académica |

La distinción no clasifica algoritmos. Aclara qué objeto estamos intentando obtener. En optimización, ese objeto es una **solución candidata**.

---

## Optimización: buscar la mejor entrada posible

En optimización conocemos el modelo y la forma de evaluar su salida. La incógnita es la **entrada** o configuración que produce el mejor resultado.

![w:430](images/optimizacion.png)

En la planificación académica:

- **Modelo:** asignación de cursos a profesores, salas y bloques.
- **Entrada buscada:** una configuración particular de esas asignaciones.
- **Salida evaluada:** cantidad de topes, incumplimientos o costos.
- **Objetivo:** encontrar la configuración con el menor costo posible.

---

## Optimización como formulación general

Muchos problemas de modelado también se entrenan reescribiéndolos como optimización:

$$
\theta^*=\arg\min_{\theta}\ \mathcal{L}(\text{datos},\,\text{modelo}_{\theta})
$$

- En una red neuronal, $\theta$ son pesos y $\mathcal{L}$ puede ser el error de predicción.
- En un sistema difuso, $\theta$ puede representar parámetros de membresía o reglas.
- En optimización de configuraciones, $\theta$ es una solución candidata, como un horario, una ruta o una configuración.

<div class="callout">
La computación evolutiva es especialmente útil cuando la función se puede evaluar, pero no es fácil derivarla, es discontinua, ruidosa o proviene de una simulación.
</div>

---

## Cuando las decisiones se combinan, el espacio crece rápido

La **optimización combinatoria** busca el mejor objeto dentro de un conjunto finito de objetos.

El conjunto puede ser finito y aun así ser imposible de recorrer exhaustivamente. Por ejemplo, ordenar $n$ actividades tiene $n!$ posibilidades.

$$
x^*=\arg\min_{x\in\Omega} f(x)
$$

- $\Omega$: soluciones admisibles o espacio de búsqueda.
- $x$: una configuración candidata.
- $f(x)$: función objetivo o medida de calidad.

La metaheurística administra un presupuesto limitado de evaluaciones de $f$.

---

## Formular la búsqueda: generar, evaluar y respetar condiciones

| Tarea | Pregunta que se responde | Ejemplo: horario académico |
|---|---|---|
| **Encontrar** | ¿Qué candidatos puedo construir? | Asignaciones de cursos, salas y bloques |
| **Evaluar** | ¿Qué tan buena es una candidatura? | Topes, capacidad, preferencias, uso de salas |
| **Aceptar** | ¿Es factible y debe conservarse? | Respeta restricciones y compite con otros |

<div class="bridge">
Una formulación explícita del espacio, la evaluación y las restricciones hace que el diseño sea discutible y reproducible.
</div>

---

## La formulación fija el significado de solución

La formulación define:

- el espacio de candidatos
- el criterio de calidad
- las restricciones de factibilidad

La misma instancia puede ser FOP, CSP o CSOP según cómo se combinen estos elementos.

---

## Objetivo y restricciones definen tres formulaciones

La diferencia entre estos nombres está en qué se considera una solución.

![w:350](images/cop.png)

| Sigla | Nombre completo | Qué se busca |
|---|---|---|
| **FOP** | Problema de Optimización Libre, *Free Optimisation Problem* | Optimizar una función sin restricciones explícitas |
| **CSP** | Problema de Satisfacción de Restricciones, *Constraint Satisfaction Problem* | Encontrar una solución que cumpla todas las restricciones |
| **CSOP / COP** | Problema de Optimización y Satisfacción de Restricciones, *Constraint Satisfaction and Optimisation Problem*. También se llama Problema de Optimización Restringido, *Constrained Optimisation Problem* | Optimizar entre las soluciones que cumplen las restricciones |

---

## Ejemplo conductor: N reinas

> Ubicar $n$ reinas en un tablero $n\times n$ sin que dos se ataquen.

![w:310](images/nqueens.png)

Dos reinas entran en conflicto si comparten fila, columna o diagonal. El ejemplo permite aplicar las tres formulaciones y luego introducir la representación y los algoritmos.

---

## La misma instancia con tres formulaciones

| Formulación | Criterio de solución | Resultado aceptado |
|---|---|---|
| **FOP** | Maximizar una función de evaluación | Tablero con el mejor valor de evaluación |
| **CSP** | Satisfacer un predicado de factibilidad | Tablero sin reinas en jaque |
| **CSOP** | Satisfacer restricciones y optimizar una evaluación | Mejor tablero entre los que cumplen las restricciones |

Las tres formulaciones usan un tablero $n\times n$ con $n$ reinas. Cambia la forma de expresar la condición de solución.

---

## N reinas como problema de optimización libre

**Representación:** tablero de ajedrez de tamaño $n\times n$ con $n$ reinas.

**Espacio de búsqueda:**

$$
S=\{s\mid s\text{ es una configuración del tablero con }n\text{ reinas}\}
$$

**Función de evaluación:** $f(s)$ es la cantidad de reinas que no están en jaque.

$$
S^*=\{s\in S\mid f(s)=n\}
$$

No hay restricciones explícitas. Los tableros con reinas en conflicto reciben una evaluación menor.

---

## N reinas como satisfacción de restricciones

**Representación y espacio:** el mismo tablero y el mismo conjunto $S$ de configuraciones con $n$ reinas.

**Predicado de factibilidad:**

$$
\operatorname{enJaque}(s)=
\begin{cases}
\text{verdadero}, & \text{si dos reinas se atacan}\\
\text{falso}, & \text{en otro caso}
\end{cases}
$$

**Soluciones:**

$$
S^*=\{s\in S\mid \operatorname{enJaque}(s)=\text{falso}\}
$$

No se ordenan las soluciones factibles. Cualquier tablero sin ataques resuelve el CSP.

---

## N reinas como optimización con restricciones

**Representación y espacio:** el mismo tablero y el mismo conjunto $S$ de configuraciones con $n$ reinas.

**Restricciones:** $\operatorname{mismaFila}(s)$ y $\operatorname{mismaColumna}(s)$ indican que existen dos reinas en una fila o columna común.

**Función de evaluación:** $D(s)$ cuenta los ataques por diagonal.

$$
\begin{aligned}
\min_{s\in S}\ & D(s)\\
\text{sujeto a}\quad & \operatorname{mismaFila}(s)=\text{falso}\\
& \operatorname{mismaColumna}(s)=\text{falso}
\end{aligned}
$$

Las restricciones conservan una reina por fila y columna. La evaluación distingue los tableros factibles según sus diagonales.

---

## La representación convierte la formulación en candidatos

La formulación define qué solución es válida. La representación define cómo codificarla.

En N reinas, un vector reduce el espacio del tablero. Las versiones siguientes difieren en cómo manejan la restricción de filas.

---

## Representación versión 1: vector con restricción explícita

Cada posición del vector representa una columna. Su valor indica la fila de la reina en esa columna:

$$
x=(x_1,\ldots,x_n),\qquad x_i\in\{1,\ldots,n\}
$$

La posición asegura una reina por columna. El espacio incluye vectores con filas repetidas:

$$
S_1=\{1,\ldots,n\}^n
$$

El predicado $\operatorname{todosDistintos}(x)$ es verdadero cuando no hay dos valores iguales. La búsqueda mantiene esa condición como una restricción explícita:

$$
\min_{x\in S_1}D(x)\qquad\text{sujeto a}\qquad \operatorname{todosDistintos}(x)=\text{verdadero}
$$

---

## Representación versión 2: espacio de permutaciones

La codificación del vector se conserva, pero el espacio de búsqueda excluye desde el inicio las filas repetidas:

$$
S_2=\{x\in\{1,\ldots,n\}^n\mid \operatorname{todosDistintos}(x)\}
=\operatorname{Perm}(\{1,\ldots,n\})
$$

La condición de fila deja de ser un predicado externo. Cada vector de $S_2$ contiene una reina por fila y columna por construcción.

$$
x^*\in\arg\min_{x\in S_2}D(x)
$$

<div class="example-space">
Para seis reinas, el vector x = [5, 3, 1, 6, 4, 2] es una permutación. Esto garantiza una reina por fila y columna. Solo falta evaluar las diagonales.
</div>

---

## Evaluar los ataques diagonales

En el vector $x$, $x_i$ es la fila de la reina situada en la columna $i$. Para dos columnas $i$ y $j$, hay ataque diagonal cuando la diferencia de filas coincide con la diferencia de columnas:

$$
C(x)=\sum_{i<j}\mathbf{1}\{|x_i-x_j|=|i-j|\}
$$

La función indicadora vale $1$ cuando el par comparte diagonal y $0$ en caso contrario. Por tanto, $C(x)$ cuenta todos los pares en conflicto.

La permutación elimina los conflictos de fila y columna por construcción. $C(x)=0$ indica una solución válida de N reinas.

---

## Del espacio de búsqueda a la estrategia de búsqueda

La formulación ya define qué candidatos son válidos y cómo se evalúan. Una estrategia de búsqueda decide qué permutaciones generar, evaluar y conservar dentro de un presupuesto limitado.

<div class="columns">
<div>

### Aproximación sistemática

- Recorre el espacio siguiendo una estrategia exhaustiva.
- Puede garantizar óptimo o demostrar que no existe solución.
- Su costo suele crecer de forma prohibitiva.

</div>
<div>

### Búsqueda local

- Examina una parte del espacio desde uno o pocos candidatos.
- Puede ser rápida y útil.
- No garantiza óptimo, factibilidad ni inexistencia de solución.

</div>
</div>

Los métodos aproximados eligen qué regiones del espacio recorrer y hacen explícito el intercambio entre calidad, costo y tiempo de cómputo.

---

## Construir o perturbar

| Enfoque | Idea | Ejemplo con N reinas |
|---|---|---|
| **Constructivo** | Crear una solución desde un estado vacío | Asignar una fila disponible a cada columna |
| **Perturbativo** | Modificar una solución existente | Intercambiar las filas de dos columnas |

En N reinas, ambos enfoques operan sobre permutaciones. Las metaheurísticas suelen construir soluciones iniciales y luego producir variaciones para buscar mejoras.

---

## Heurísticas: reglas que aprovechan estructura

Una heurística es una regla práctica que orienta una decisión con información del problema.

Ejemplo de estrategia voraz para N reinas: asignar una fila disponible a cada columna, eligiendo la que produzca menos conflictos diagonales con las reinas ya ubicadas.

| Ventaja | Límite |
|---|---|
| Puede obtener una solución razonable muy rápido. | Una decisión localmente buena puede bloquear una solución global mejor. |

<div class="bridge">
Una heurística puede generar una buena población inicial. También puede complementar el trabajo de una metaheurística.
</div>

---

## Heurística para 2048

![w:430](images/2048.png)

Una regla requiere precisar el objetivo. Puede ser obtener la ficha de mayor valor, mantener casillas libres o prolongar la partida.

Una heurística posible es conservar las fichas de mayor valor en una esquina y evitar movimientos que cierren el tablero.

La regla usa conocimiento del juego y permite decidir con rapidez. Sin embargo, una buena decisión inmediata no garantiza el mejor resultado al final de la partida.

---

## Metaheurísticas: estrategias de búsqueda reutilizables

> Proceso automatizado para encontrar una buena solución en un espacio grande, construyéndola o mejorándola, sin garantía de óptimo global.

No codifican reglas específicas de un único dominio. En cambio, establecen cómo generar, evaluar, aceptar y diversificar candidatos.

| Heurística | Metaheurística |
|---|---|
| Decide con conocimiento particular del problema. | Define una estrategia de búsqueda aplicable a distintas formulaciones. |
| Ej.: priorizar asignaturas con mayor demanda. | Ej.: búsqueda local, recocido simulado o evolución. |

---

## Lenguaje mínimo de búsqueda local

- **Candidato:** solución que puede evaluarse.
- **Movimiento o perturbación:** cambio aplicado a un candidato.
- **Vecindario $N(x)$:** conjunto de candidatos alcanzables con un movimiento.
- **Criterio de selección:** decide qué vecino pasa a ser el siguiente candidato.
- **Reinicio:** introduce una nueva región cuando se detecta estancamiento.
- **Criterio de término:** limita evaluaciones, iteraciones, tiempo o ausencia de mejora.

La búsqueda evolutiva conserva este vocabulario, pero trabaja con una **población** y no con un único candidato.

---

## Exploración y explotación

![w:610](images/convergence-ea.png)

- **Explotar (intensificar):** refinar regiones que ya parecen prometedoras.
- **Explorar (diversificar):** visitar regiones nuevas del espacio de búsqueda.

Explotar demasiado lleva a convergencia prematura en un óptimo local. Explorar demasiado impide consolidar mejoras. El equilibrio depende del problema, la representación y el presupuesto.

---

## Metaheurísticas clásicas

| Método | Mecanismo para evitar o manejar óptimos locales |
|---|---|
| Hill-Climbing | Acepta mejoras en el vecindario. Puede detenerse en un óptimo local. |
| Iterative Local Search | Perturba la solución y vuelve a ejecutar una búsqueda local. |
| Tabu Search | Usa memoria para evitar movimientos o soluciones visitadas recientemente. |
| Simulated Annealing | Acepta a veces una solución peor para escapar de un óptimo local. |
| GRASP (Greedy Randomized Adaptive Search Procedure) | Construye una solución voraz con decisiones aleatorias y luego la mejora localmente. |

<div class="callout">
El objetivo es reconocer cómo cada método administra exploración, explotación y presupuesto.
</div>

---

## Optimización con varios objetivos

En planificación académica, minimizar topes puede entrar en conflicto con minimizar salas o maximizar preferencias. Una suma ponderada los combina:

$$
f(x)=w_1 f_1(x)+w_2 f_2(x)+\cdots+w_m f_m(x)
$$

Los pesos incorporan una preferencia que debe justificarse. La optimización con varios objetivos mantiene el conflicto explícito y busca un conjunto de soluciones de compromiso.

---

## Dominancia: comparar soluciones objetivo por objetivo

En minimización, $x$ domina a $y$ cuando no es peor en ningún objetivo y es mejor en al menos uno:

$$
f_i(x)\le f_i(y)\ \forall i
\qquad\land\qquad
\exists j:\ f_j(x)<f_j(y)
$$

| Solución | $f_1$ | $f_2$ |
|---|---:|---:|
| $A$ | 2 | 8 |
| $B$ | 3 | 8 |
| $C$ | 1 | 10 |

$A$ domina a $B$: mejora $f_1$ y mantiene $f_2$. Entre $A$ y $C$ no hay dominancia: $C$ mejora $f_1$, pero empeora $f_2$.

---

## Frente de Pareto: soluciones no dominadas

El frente de Pareto reúne las soluciones que no son dominadas por ninguna otra:

$$
\mathcal{P}=\{x\in\Omega\mid \nexists y\in\Omega:\ y\text{ domina a }x\}
$$

![w:330](images/multi-objective.png)

En el gráfico, los puntos azules son soluciones evaluadas, los rojos forman el frente y el verde es una solución encontrada sobre él.

En el frente, mejorar un objetivo exige empeorar al menos otro. Elegir una solución requiere preferencias externas al algoritmo.

---

## Parámetros: definirlos antes o durante la búsqueda

![w:500](images/parameterconfig.png)

- **Ajuste previo:** los valores se fijan antes de ejecutar, mediante pruebas o conocimiento previo.
- **Control durante la ejecución:** los valores cambian mientras el algoritmo trabaja.
  - *Determinista:* sigue una regla programada.
  - *Adaptativo:* responde al desempeño observado.
  - *Autoadaptativo:* los parámetros evolucionan junto con las soluciones.

La elección entre ajuste previo y control durante la ejecución afecta la mutación, el tamaño de población y la presión de selección.

---

## La transición clave: ¿por qué una población?

Una búsqueda local ve principalmente el vecindario de un candidato. Una población permite mantener varias hipótesis de solución al mismo tiempo.

| Enfoque | Evolución de la búsqueda |
|---|---|
| Búsqueda local | Un candidato → una trayectoria |
| Búsqueda poblacional | Una población → varias trayectorias |

<div class="bridge">
La computación evolutiva usa diversidad poblacional como memoria de regiones distintas y la selección como mecanismo para concentrar recursos en las más prometedoras.
</div>

---

## Idea central

> Las técnicas evolutivas son metaheurísticas inspiradas en la evolución natural. Transforman una población de soluciones mediante selección, variación y supervivencia.

La evolución natural aporta la idea general. Sin embargo, la evaluación sigue siendo una decisión de ingeniería. La **aptitud** define qué significa que una solución esté bien adaptada al problema.

| Biología | Optimización |
|---|---|
| Individuo | Solución candidata |
| Ambiente | Función de evaluación y restricciones |
| Reproducción | Generación de descendencia |
| Adaptación | Mejor desempeño según el objetivo |

---

## Ciclo general de un algoritmo evolutivo

![w:560](images/ea-workflow.png)

$$
P_t\longrightarrow P'_t\longrightarrow O_t\longrightarrow P_{t+1}
$$

Selección de padres → variación → evaluación y supervivencia

Cada flecha representa una decisión. Cambiar la representación, los operadores o el reemplazo cambia el comportamiento del algoritmo.

---

## Pseudocódigo: el orden importa

```text
P ← inicializar población
evaluar P
mientras no se cumpla el criterio de término:
    padres ← seleccionar(P)
    hijos ← variar(padres)          # recombinación y/o mutación
    evaluar hijos
    P ← seleccionar_supervivientes(P, hijos)
retornar mejor solución observada
```

La calidad que vemos al final depende de **toda** esta cadena. Por eso, “usar un algoritmo genético” no es una especificación suficiente para reproducir un resultado.

---

## Genotipo, fenotipo y representación

<div class="columns">
<div>

### Fenotipo

La solución interpretada en el dominio: tablero, horario, ruta o asignación de recursos.

### Genotipo

La codificación manipulada por el algoritmo: bits, vector real, permutación o árbol.

</div>
<div>

![w:400](images/phenotype.png)

**Codificar** lleva del fenotipo al genotipo. **Decodificar** permite interpretar un genotipo en el problema real.

</div>
</div>

---

## Componentes de un individuo

Para el vector de seis reinas $X=[5,3,1,6,4,2]$:

- **Individuo o cromosoma:** el vector completo $X$.
- **Gen:** una posición, por ejemplo $X[0]$, asociada a la primera columna.
- **Alelo:** su valor particular, aquí $5$, que indica la fila de esa reina.

<div class="callout">
Estos nombres ayudan a describir los operadores. Cada cambio debe tener una interpretación útil y debe preservar o reparar las restricciones necesarias.
</div>

---

## Una representación útil conecta con sus operadores

Una codificación debería:

- cubrir todas las soluciones relevantes.
- evitar, reparar o penalizar candidatos inválidos.
- admitir variaciones pequeñas con significado.
- permitir una evaluación eficiente.
- evitar redundancia innecesaria.

<div class="warn">
Un cruzamiento de un punto sobre dos permutaciones puede repetir filas y eliminar otras. Para N reinas se requieren operadores de permutación o una estrategia explícita de reparación.
</div>

---

## Aptitud: medir la calidad de cada individuo

La función de **aptitud** o *fitness* permite comparar individuos. Para N reinas podemos minimizar directamente los conflictos diagonales:

$$
f(x)=C(x)
$$

Si una implementación requiere maximizar, una transformación posible es:

$$
F(x)=\frac{1}{1+C(x)}
$$

La transformación debe preservar el orden relevante y no exagerar diferencias numéricas. En DEAP, el signo de los pesos permite declarar minimización o maximización sin cambiar artificialmente el objetivo.

---

## Población y diversidad

Una población es un conjunto de candidatos en el que puede haber elementos repetidos. Su tamaño suele fijarse, pero una población grande no garantiza diversidad por sí sola.

| Dónde medir diversidad | Pregunta |
|---|---|
| Genotipo | ¿Los cromosomas son distintos? |
| Fenotipo | ¿Representan soluciones distintas? |
| Aptitud | ¿Obtienen calidades distintas? |

Varios cromosomas diferentes pueden representar el mismo fenotipo. Por eso, la medida de diversidad debe corresponder al problema.

---

## Diversidad: lo que conviene monitorear

Para una población binaria, la entropía en la posición $j$ puede expresarse como:

$$
H_j=-p_j\log p_j-(1-p_j)\log(1-p_j)
$$

También se pueden usar distancia de Hamming, distancia euclidiana, individuos únicos o diversidad de objetivos.

<div class="bridge">
Registrar solo la mejor aptitud no permite saber si la población convergió de forma adecuada o perdió diversidad demasiado pronto.
</div>

---

## Operadores: tres decisiones diferentes

| Tipo | Pregunta | Ejemplos |
|---|---|---|
| Selección de padres | ¿Quién puede producir descendencia? | ruleta, ordenamiento, torneo |
| Variación | ¿Cómo se producen nuevos candidatos? | recombinación, mutación |
| Supervivencia | ¿Quién queda disponible en la próxima generación? | elitismo, ordenamiento, edad |

Separar estas etapas evita una confusión frecuente: ser elegido como padre no implica sobrevivir, y sobrevivir no implica tener más hijos.

---

## Selección de padres

| Método | Idea | Riesgo o control |
|---|---|---|
| Aleatoria | No usa la aptitud | Sirve como referencia, pero no aprovecha la calidad |
| Ruleta | Probabilidad proporcional a la aptitud | Es sensible a la escala y a valores extremos |
| Ordenamiento | Probabilidad según la posición ordenada | Controla la presión, pero pierde la magnitud absoluta |
| Torneo | Muestrea $k$ y escoge el mejor | $k$ regula la presión |

![w:200](images/roulettewheel.png)

Un torneo más grande aumenta la presión de selección y puede reducir la diversidad.

---

## Recombinación: combinar información útil

La recombinación toma dos o más padres para formar descendencia. Es útil solo si los fragmentos heredados conservan significado en la representación.

![w:460](images/recombination.png)

- Cadenas binarias: uno o varios puntos de corte.
- Vectores reales: combinaciones aritméticas o intermedias.
- Permutaciones: PMX, OX o CX, que evitan duplicados.

En N reinas, OX conserva un segmento de un padre y completa el resto según el orden del otro. El resultado sigue siendo una permutación válida.

---

## Mutación: recuperar y abrir alternativas

La mutación es una perturbación estocástica aplicada a un individuo.

![w:470](images/mutation.png)

| Representación | Mutaciones habituales |
|---|---|
| Bits | inversión de un bit |
| Vectores reales | ruido gaussiano |
| Permutaciones | intercambio, inserción, inversión |

Una tasa muy alta se aproxima al muestreo aleatorio. Una tasa muy baja puede congelar la población. También se debe distinguir entre la probabilidad de mutar un individuo y la probabilidad de modificar cada gen.

---

## Recombinación y mutación cumplen roles complementarios

### Padres → recombinación → mutación → variantes evaluables

- La recombinación reutiliza y combina información ya presente en la población.
- La mutación introduce alternativas que la recombinación por sí sola podría no alcanzar.
- Ninguna garantiza una mejora. La aptitud se conoce después de evaluar.

<div class="callout">
En el lenguaje de las metaheurísticas, ambos son mecanismos de perturbación. La selección y el reemplazo determinan cuánto se aprovechan las soluciones que funcionaron bien.
</div>

---

## Selección de supervivientes

![w:420](images/survivalrank.png)

- **Reemplazo generacional:** la descendencia sustituye toda la población.
- **Reemplazo estacionario:** se cambian pocos individuos por iteración.
- **Ordenamiento por aptitud:** padres e hijos compiten y permanecen los mejores.
- **Edad:** se limita cuántas generaciones puede permanecer un individuo.
- **Elitismo:** se preserva explícitamente uno o más mejores individuos.

---

## Elitismo: ventajas y riesgos

Con elitismo, el mejor valor observado no empeora entre generaciones, porque al menos un buen individuo se conserva.

Sin embargo, demasiado elitismo produce copias y reduce diversidad:

**Conservar el mejor valor ↔ mantener diversidad para adaptarse**

La elección del elitismo depende de cuántos individuos se preservan y de qué mecanismo mantiene suficiente exploración.

---

## Cómo empezar y cómo terminar

<div class="columns">
<div>

### Inicialización

![w:330](images/initialpop.png)

- aleatoria, para cubrir distintas regiones.
- heurística, para incluir buenas soluciones conocidas.
- híbrida, para equilibrar ambas.

</div>
<div>

### Término

- presupuesto de evaluaciones.
- número de generaciones.
- tiempo máximo.
- calidad objetivo.
- estancamiento o pérdida de diversidad.

La comparación entre configuraciones requiere el mismo presupuesto de **evaluaciones de aptitud**.

</div>
</div>

---

## Cuatro familias históricas

![w:600](images/ea.png)

Las fronteras modernas son flexibles. Aun así, las familias ayudan a reconocer combinaciones frecuentes de representación y operadores.

---

## Comparación de familias evolutivas

| Familia | Representación típica | Variación característica | Énfasis |
|---|---|---|---|
| Programación evolutiva (EP) | Vectores reales | Mutación gaussiana | Predicción y competencia, con poca recombinación en su forma histórica |
| Estrategias evolutivas (ES) | Vectores reales y parámetros | Recombinación y mutación | Autoadaptación y esquemas $(\mu,\lambda)$ / $(\mu+\lambda)$ |
| Algoritmos genéticos (GA) | Cadenas, luego representaciones generales | Cruzamiento y mutación | Generaciones, selección y recombinación |
| Programación genética (GP) | Árboles o programas | Intercambio y cambio de subárboles | Evolución de expresiones o programas |

<div class="bridge">
La familia por sí sola no permite reproducir un método. Es necesario especificar la representación y los operadores concretos.
</div>

---

## Estrategias de evolución: dos esquemas de supervivencia

Con $\mu$ padres y $\lambda$ descendientes:

| Esquema | Supervivientes |
|---|---|
| $(\mu,\lambda)$ | Solo compiten los $\lambda$ hijos. Los padres desaparecen. |
| $(\mu+\lambda)$ | Padres e hijos compiten juntos. Este esquema permite elitismo. |

Esta distinción conecta directamente con la selección de supervivientes vista antes: el diseño del reemplazo modifica tanto presión de selección como diversidad.

---

## Derivaciones y métodos inspirados en la naturaleza

<div class="columns">
<div>

### Coevolución

Una o varias poblaciones se evalúan en interacción. Puede modelar cooperación (simbiosis) o competencia (parasitismo).

La calidad de una solución puede depender de con quién se compara, no solo de una función fija.

</div>
<div>

### Otras metaheurísticas poblacionales

- **Ant Colony Optimization:** la información colectiva se expresa mediante feromonas.
- **Particle Swarm Optimization:** los candidatos se mueven guiados por experiencia propia y del grupo.

Comparten la idea poblacional, pero sus mecanismos no son los de un algoritmo genético.

</div>
</div>

---

## Algoritmo evolutivo para N reinas

Diseño de un EA que encuentre una permutación con $C(x)=0$.

| Decisión | Propuesta inicial razonable |
|---|---|
| Individuo | Permutación de $\{1,\ldots,n\}$ |
| Aptitud | Número de conflictos diagonales $C(x)$, a minimizar |
| Padres | Torneo |
| Recombinación | OX u otro operador de permutaciones |
| Mutación | Intercambio de dos posiciones |
| Supervivencia | Generacional con elitismo moderado |
| Término | Éxito, presupuesto de evaluaciones o estancamiento |

No son elecciones universales. Son hipótesis de diseño que deben evaluarse.

---

## Implementación: la biblioteca no reemplaza el diseño

El material práctico complementario, [`03_DEAP.ipynb`](03_DEAP.ipynb) y [`notebook/03_ga_n_reinas.ipynb`](notebook/03_ga_n_reinas.ipynb), permite implementar el ciclo usando DEAP.

DEAP separa:

- definición de individuos y aptitud.
- registro de operadores.
- ejecución del ciclo evolutivo.
- estadísticas y registro experimental.

<div class="warn">
La biblioteca puede ejecutar los operadores, pero no decide una representación válida, qué objetivos importan ni si una comparación experimental es justa.
</div>

---

## Protocolo experimental mínimo

1. Fijar instancia, restricciones y presupuesto de evaluaciones.
2. Definir una línea base: muestreo aleatorio o heurística simple.
3. Ejecutar varias semillas independientes.
4. Registrar la mejor aptitud, el promedio, la dispersión y la diversidad por generación.
5. Reportar éxito, costo computacional y distribución de resultados.
6. Comparar curvas respecto de evaluaciones, no solo respecto de generaciones.

La tasa de éxito es la proporción de ejecuciones que alcanzan $C(x)=0$.

---

## Qué significa una comparación justa

| Práctica insuficiente | Práctica recomendable |
|---|---|
| Informar la mejor corrida. | Informar mediana, dispersión y tasa de éxito. |
| Comparar generaciones con poblaciones de tamaños distintos. | Igualar evaluaciones de aptitud o declarar claramente el presupuesto. |
| Cambiar todos los parámetros a la vez. | Realizar análisis de sensibilidad controlado. |
| Usar una semilla implícita. | Registrar semillas, instancia y configuración. |

<div class="callout">
Un algoritmo evolutivo es estocástico. Una ejecución aislada solo muestra un resultado posible. Varias ejecuciones permiten estudiar el comportamiento habitual y su variabilidad.
</div>

---

## Casos para analizar

1. ¿Qué se pierde y qué se gana al restringir N reinas a permutaciones?
2. ¿Qué pasaría si usamos cruzamiento de un punto sin reparación?
3. ¿Cómo se comparan población 50 por 100 generaciones y población 100 por 50 generaciones?
4. ¿Qué señal mostraría que el elitismo está causando convergencia prematura?
5. En un horario, ¿qué restricciones son duras y qué preferencias deberían ser objetivos?

Estos casos conectan la implementación con decisiones de modelado.

---

## Síntesis

$$
\text{formular}
\rightarrow
\text{representar}
\rightarrow
\text{evaluar}
\rightarrow
\text{variar y seleccionar}
\rightarrow
\text{medir con evidencia}
$$

- Un algoritmo evolutivo administra un presupuesto de evaluaciones sobre una población.
- Representación, aptitud, operadores y supervivencia forman un diseño inseparable.
- La selección impulsa la explotación. La variación y la diversidad sostienen la exploración.
- Los resultados deben evaluarse con repeticiones, líneas base y presupuestos comparables.

<div class="bridge">
El ajuste de parámetros de modelos a partir de datos también puede formularse como un problema de optimización.
</div>

---

## Referencias y atribución del material

### Material de referencia

- **Gálvez Ramírez, Nicolás.** *Introducción a la Computación Evolutiva*, material docente de IPD434, Universidad Técnica Federico Santa María. Archivo base: [`03_ComputacionEvolutiva.ipynb`](03_ComputacionEvolutiva.ipynb).
- Este documento adapta, reorganiza y amplía ese material para fortalecer las conexiones conceptuales.
- Material práctico complementario: [`03_DEAP.ipynb`](03_DEAP.ipynb) y [`notebook/03_ga_n_reinas.ipynb`](notebook/03_ga_n_reinas.ipynb).

---

## Referencias bibliográficas

- Eiben, A. E. y Smith, J. E. *Introduction to Evolutionary Computing*, 2.ª ed., Springer, 2015.
- Goldberg, D. E. *Genetic Algorithms in Search, Optimization, and Machine Learning*, Addison-Wesley, 1989.
- Deb, K. *Multi-Objective Optimization Using Evolutionary Algorithms*, Wiley, 2001.
- Fortin, F.-A. et al. “DEAP: Evolutionary Algorithms Made Easy”, *Journal of Machine Learning Research*, 2012.
