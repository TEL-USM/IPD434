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
## Introducción a Soft Computing
### Complejidad, búsqueda y aproximación

Dr. Nicolás Gálvez Ramírez<br>
Dr. Patricio Olivares Roncagliolo

---

## Conexión con el curso

La presentación del curso planteó tres mecanismos:

$$
\text{imprecisión}\rightarrow\text{lógica difusa},\qquad
\text{búsqueda}\rightarrow\text{evolución},\qquad
\text{datos}\rightarrow\text{redes neuronales}
$$

Esta unidad explica la motivación común: el costo de obtener una solución exacta o de construir un modelo preciso puede ser prohibitivo.

<div class="bridge">
Antes de elegir una técnica se debe entender qué hace difícil al problema.
</div>

---

## Objetivos de la unidad

Al finalizar esta unidad se espera poder:

1. **Explicar** qué caracteriza a Soft Computing.
2. **Representar** un problema mediante estados, soluciones y transformaciones.
3. **Distinguir** búsqueda completa e incompleta.
4. **Relacionar** computabilidad y complejidad con la necesidad de aproximar.
5. **Diferenciar** las clases P, NP, NP-completo y NP-hard.
6. **Justificar** una técnica aproximada sin confundir viabilidad con optimalidad.

---

## Soft Computing

El término fue introducido por Lotfi A. Zadeh para agrupar técnicas tolerantes a:

- imprecisión;
- incertidumbre;
- verdad parcial;
- aproximación.

<div class="callout">
La meta es obtener soluciones tratables, robustas y de bajo costo cuando una formulación exacta resulta innecesaria o impracticable.
</div>

Soft Computing complementa a los métodos exactos; no afirma que toda aproximación sea aceptable.

La aproximación se justifica cuando reduce el costo o permite tratar la incertidumbre sin perder la calidad necesaria para la decisión.

---

## Tres ejemplos cotidianos

<div class="columns">
<div>

**Estacionar un automóvil**

Se decide con estimaciones graduales: “muy cerca”, “ángulo suficiente”, “girar poco”.

**Reconocer escritura**

Se clasifica aunque cada carácter difiera de una plantilla ideal.

</div>
<div>

**Buscar alimento**

Una colonia explora y refuerza rutas sin un controlador central.

**Idea común**

La solución emerge desde información parcial, experiencia o interacción.

</div>
</div>

---

## Componentes principales

| Familia | Representación | Mecanismo | Resultado |
|---|---|---|---|
| Lógica difusa | Grados de pertenencia y reglas | Inferencia | Decisión gradual |
| Redes neuronales | Parámetros y capas | Optimización desde datos | Función aprendida |
| Algoritmos evolutivos | Población de candidatos | Selección y variación | Solución aproximada |
| Sistemas híbridos | Combinación de las anteriores | Aprendizaje e inferencia | Compromiso entre capacidades |

---

## Representar un problema de búsqueda

Una formulación mínima contiene:

- un espacio de estados $S$;
- un estado inicial $s_0$;
- un conjunto de estados objetivo $G\subseteq S$;
- acciones o transformaciones $A(s)$;
- un costo o función objetivo $f:S\rightarrow\mathbb{R}$.

La dificultad depende tanto de $|S|$ como del costo de evaluar, generar y comparar candidatos.

Un estado describe una configuración posible; una solución es un estado que satisface el objetivo y las restricciones. No todos los estados tienen que ser soluciones válidas.

---

## Ejemplo: tres en línea

![w:560](images/tictactoe.png)

Cada jugada transforma un estado del tablero. Aun en este caso pequeño aparecen:

- estados válidos e inválidos;
- estados terminales;
- ramas equivalentes por simetría;
- decisiones condicionadas por un adversario.

<div class="example-space">
Actividad: proponga una codificación de estado y una condición de término.
</div>

---

## Crecimiento del espacio

Si una solución tiene $n$ posiciones y cada una admite $k$ valores, el número bruto de configuraciones es:

$$
|S|=k^n
$$

Para un recorrido por $n$ ciudades:

$$
|S|=(n-1)!/2
$$

considerando una ciudad inicial fija y recorridos inversos equivalentes.

Por ejemplo, con $n=20$ existen aproximadamente $6.1\times10^{16}$ recorridos distintos bajo esas equivalencias. Enumerarlos deja de ser práctico aunque evaluar un recorrido individual sea sencillo.

<div class="warn">
Un computador más rápido no elimina el crecimiento exponencial o factorial; solo desplaza el tamaño de instancia alcanzable.
</div>

---

## Búsqueda completa e incompleta

<div class="columns">
<div>

**Completa**

- Puede demostrar optimalidad o inexistencia.
- Recorre sistemáticamente el espacio relevante.
- Su costo puede crecer de manera prohibitiva.

</div>
<div>

**Incompleta**

- Explora una fracción del espacio.
- Depende de heurísticas y parámetros.
- Busca calidad suficiente con recursos limitados.

</div>
</div>

Una técnica incompleta debe reportar calidad, variabilidad y presupuesto computacional.

“Completa” describe una garantía de cobertura o solución, no necesariamente un algoritmo rápido. “Incompleta” indica que el método puede finalizar sin demostrar optimalidad o inexistencia.

---

## Máquina de Turing

El modelo de Turing formaliza una máquina con cinta, cabezal, estados y reglas de transición.

$$
\delta:Q\times\Gamma\rightarrow\Gamma\times\{L,R\}\times Q
$$

donde $Q$ es el conjunto de estados y $\Gamma$ el alfabeto de la cinta.

![w:470](images/cinta_turing.png)

El artículo original de Turing de 1936 está disponible en `../../Material/Turing_Paper_1936.pdf`.

---

## Determinismo y no determinismo

Una máquina determinista tiene a lo sumo una transición aplicable por configuración.

Una máquina no determinista admite un conjunto de transiciones:

$$
\delta:Q\times\Gamma\rightarrow
\mathcal{P}\!\left(\Gamma\times\{L,R\}\times Q\right)
$$

Una entrada se acepta si al menos una rama alcanza un estado de aceptación.

<div class="callout">
El no determinismo es un recurso matemático para definir complejidad; no es una metaheurística ni una máquina física que pruebe todas las ramas gratis.
</div>

---

## Clase P

P reúne problemas de decisión resolubles por una máquina determinista en tiempo polinómico:

$$
T(n)\in O(n^k),\qquad k\text{ constante}
$$

Interpretación práctica:

- existe un algoritmo cuyo crecimiento se considera tratable en el modelo teórico;
- “polinómico” no garantiza que toda instancia sea barata;
- la representación de la entrada y el tamaño $n$ deben declararse.

P es una clase teórica de problemas de decisión. Que un algoritmo sea polinómico no implica automáticamente que resulte conveniente para todos los tamaños o constantes involucradas.

---

## Clase NP

NP reúne problemas de decisión cuyas instancias afirmativas poseen un certificado verificable en tiempo polinómico.

Equivalentemente, pueden resolverse en tiempo polinómico por una máquina de Turing no determinista.

$$
P\subseteq NP
$$

<div class="warn">
NP significa “polinómico no determinista”, no “no polinómico”. Tampoco implica que verificar cualquier respuesta negativa sea fácil.
</div>

La definición basada en certificados suele ser la más operativa: si alguien entrega una solución afirmativa, debe existir una verificación eficiente de que realmente cumple.

---

## NP-completo y NP-hard

Un problema $A$ es **NP-completo** si:

1. $A\in NP$;
2. todo problema $B\in NP$ se reduce a $A$ en tiempo polinómico.

Un problema es **NP-hard** si todo problema de NP se reduce a él, aunque no sea un problema de decisión ni pertenezca a NP.

$$
\text{NP-completo}=NP\cap\text{NP-hard}
$$

---

## Decisión y optimización en TSP

![w:440](images/tsp.png)

- **Decisión:** ¿existe un tour de costo menor o igual que $K$?
- **Optimización:** ¿cuál es el tour de costo mínimo?
- **Verificación:** dado un tour, su costo se calcula en tiempo polinómico.

El problema de decisión es NP-completo; la versión de optimización es NP-hard.

Esta distinción importa al comunicar resultados: una metaheurística puede mejorar recorridos, pero no demuestra que el mejor recorrido hallado sea óptimo.

---

## ¿P = NP?

![w:570](images/p_np.svg)

No se conoce si $P=NP$. Si un problema NP-completo tuviera un algoritmo determinista polinómico, entonces todos los problemas de NP también lo tendrían.

<div class="bridge">
Soft Computing no resuelve P versus NP. Ofrece estrategias prácticas para buscar, aprender o decidir cuando el método exacto no es viable dentro del presupuesto disponible.
</div>

---

## Cómo justificar una aproximación

1. Definir calidad de solución y restricciones.
2. Identificar el tamaño y estructura del espacio.
3. Explicar por qué el método exacto no es viable o necesario.
4. Fijar un presupuesto de tiempo o evaluaciones.
5. Comparar con una línea base.
6. Repetir el experimento si existe aleatoriedad.
7. Reportar el mejor resultado y su distribución.

$$
\text{valor de la aproximación}
=\text{calidad obtenida bajo recursos explícitos}
$$

El presupuesto puede expresarse en tiempo, memoria o número de evaluaciones. Para comparar métodos, debe mantenerse equivalente.

---

## Síntesis

- Soft Computing tolera imprecisión y aproximación para lograr tratabilidad y robustez.
- El espacio de búsqueda y la representación condicionan la dificultad.
- P, NP, NP-completo y NP-hard describen relaciones formales entre problemas.
- Una heurística produce candidatos; la evidencia determina si son útiles.
- La aproximación debe justificarse con métricas y presupuesto.

<div class="bridge">
La próxima unidad reemplaza la verdad binaria por grados de pertenencia para representar conceptos lingüísticos e imprecisos.
</div>

---

## Referencias y material complementario

- Material original de IPD434, notebook `01_IntroduccionSoftComputing.ipynb`.
- A. M. Turing, “On Computable Numbers, with an Application to the Entscheidungsproblem”, 1936. Disponible en `../../Material/`.
- S. A. Cook, “The Complexity of Theorem-Proving Procedures”, 1971.
- R. M. Karp, “Reducibility Among Combinatorial Problems”, 1972.
- L. A. Zadeh, “Fuzzy Logic, Neural Networks, and Soft Computing”, 1994.
