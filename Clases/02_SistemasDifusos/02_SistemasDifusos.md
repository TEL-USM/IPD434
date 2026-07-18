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
## Sistemas difusos
### De conceptos lingüísticos a decisiones numéricas

Dr. Nicolás Gálvez Ramírez<br>
Dr. Patricio Olivares Roncagliolo

---

## Conexión con la unidad anterior

Soft Computing admite soluciones aproximadas cuando una frontera exacta no representa bien el problema.

¿A qué temperatura una habitación deja de ser “templada” y pasa a ser “caliente”?

<div class="columns">
<div>

**Conjunto clásico**

$$
\chi_A(x)\in\{0,1\}
$$

</div>
<div>

**Conjunto difuso**

$$
\mu_A(x)\in[0,1]
$$

</div>
</div>

<div class="bridge">
La lógica difusa modela gradualidad. No convierte incertidumbre aleatoria en probabilidad.
</div>

---

## Objetivos de la unidad

Al finalizar se espera poder:

1. **Definir** conjuntos difusos y funciones de pertenencia.
2. **Aplicar** operaciones, relaciones y composición max–min.
3. **Construir** variables lingüísticas y reglas difusas.
4. **Explicar** las etapas de un sistema de inferencia.
5. **Calcular** una salida mediante agregación y desfusificación.
6. **Implementar y validar** un controlador Mamdani con Scikit-Fuzzy.

---

## Ruta de la clase

1. Representamos conceptos graduales con funciones de pertenencia.
2. Combinamos esos grados mediante operaciones y relaciones.
3. Expresamos conocimiento con variables lingüísticas y reglas.
4. Activamos y agregamos reglas para construir una salida difusa.
5. Desfusificamos la salida y validamos el comportamiento en todo el dominio.

<div class="bridge">
Cada etapa transforma la representación anterior. Omitir una etapa impide explicar de dónde proviene la decisión numérica final.
</div>

---

## Conjunto difuso

Un conjunto difuso $A$ sobre un universo $X$ se representa por:

$$
A=\{(x,\mu_A(x))\mid x\in X\}
$$

donde $\mu_A(x)$ expresa el grado en que $x$ satisface el concepto representado por $A$.

Ejemplo: para el concepto “temperatura alta”, $\mu_{alta}(28)=0.7$ significa pertenencia parcial, no una probabilidad de $70\%$.

La función de pertenencia se define para un concepto y un contexto concretos: $28\ ^\circ\mathrm{C}$ puede ser “alto” para una habitación, pero no para un horno industrial.

---

## Propiedades descriptivas

Para un conjunto difuso $A$:

- **Soporte:** $\operatorname{supp}(A)=\{x\mid\mu_A(x)>0\}$.
- **Núcleo:** $\operatorname{core}(A)=\{x\mid\mu_A(x)=1\}$.
- **Altura:** $h(A)=\sup_x\mu_A(x)$.
- **Normalidad:** $A$ es normal si $h(A)=1$.
- **Corte $\alpha$:** $A_\alpha=\{x\mid\mu_A(x)\ge\alpha\}$.

Los cortes $\alpha$ permiten estudiar un conjunto difuso mediante familias de conjuntos clásicos.

Por ejemplo, el corte $A_{0.8}$ reúne los elementos que cumplen el concepto con grado al menos $0.8$. Al aumentar $\alpha$, el conjunto resultante no puede crecer.

---

## Funciones de pertenencia frecuentes

<div class="columns">
<div>

**Triangular**

$$
\mu(x;a,b,c)=\max\!\left(\min\!\left(\frac{x-a}{b-a},\frac{c-x}{c-b}\right),0\right)
$$

**Trapezoidal**

$$
\mu(x)=\max\!\left(\min\!\left(\frac{x-a}{b-a},1,\frac{d-x}{d-c}\right),0\right)
$$

</div>
<div>

**Gaussiana**

$$
\mu(x;c,\sigma)=\exp\!\left[-\frac{(x-c)^2}{2\sigma^2}\right]
$$

La forma debe representar conocimiento o datos y luego validarse. No se elige solo por conveniencia gráfica.

Los parámetros $a,b,c,d$ controlan los puntos de inicio, pertenencia plena y término. Los parámetros $c$ y $\sigma$ controlan el centro y la dispersión de una gaussiana.

</div>
</div>

---

## Operaciones básicas

Con los operadores estándar de Zadeh:

$$
\mu_{A\cup B}(x)=\max(\mu_A(x),\mu_B(x))
$$

$$
\mu_{A\cap B}(x)=\min(\mu_A(x),\mu_B(x))
$$

$$
\mu_{\bar A}(x)=1-\mu_A(x)
$$

Existen otras t-normas y t-conormas. Cambiar el operador cambia la semántica de “y” u “o” y, por tanto, la salida del sistema.

Con mínimo y máximo, la intersección queda limitada por el antecedente menos satisfecho y la unión conserva el grado mayor.

---

## Transformaciones de pertenencia

El material base incluye operaciones que modifican el énfasis de un conjunto:

- Concentración: $\mu_{CON(A)}(x)=\mu_A(x)^2$.
- Dilatación: $\mu_{DIL(A)}(x)=\sqrt{\mu_A(x)}$.
- Complemento: $\mu_{\bar A}(x)=1-\mu_A(x)$.

En términos lingüísticos, modificadores como “muy” o “más o menos” pueden modelarse mediante estas transformaciones.

La concentración reduce los grados intermedios y vuelve el concepto más exigente. La dilatación los aumenta y produce una interpretación más amplia.

---

## Variables lingüísticas

Una variable lingüística se describe mediante:

$$
V=(X,T(X),U,G,M)
$$

donde $X$ es el nombre, $T(X)$ sus términos, $U$ el universo, $G$ una gramática y $M$ la semántica que asigna funciones de pertenencia.

Ejemplo:

- variable: temperatura.
- universo: $U=[10,40]\ ^\circ\mathrm{C}$.
- términos: baja, confortable, alta.

---

## Relaciones difusas

Una relación difusa $R$ sobre $X\times Y$ asigna:

$$
\mu_R:X\times Y\rightarrow[0,1]
$$

Para relaciones $R(X,Y)$ y $S(Y,Z)$, la composición max–min es:

$$
\mu_{R\circ S}(x,z)=\max_{y\in Y}
\min\left(\mu_R(x,y),\mu_S(y,z)\right)
$$

El mínimo mide compatibilidad a través de $y$. El máximo conserva el camino más compatible.

En una representación matricial, para cada par $(x,z)$ se comparan todos los valores intermedios $y$. El resultado conserva la mejor conexión disponible entre ambos extremos.

---

## Principio de extensión

El principio de extensión permite aplicar una función clásica $y=f(x)$ a una entrada difusa $A$:

$$
\mu_B(y)=\sup_{x:f(x)=y}\mu_A(x)
$$

Para varias entradas y t-norma mínimo:

$$
\mu_B(y)=\sup_{f(x_1,\dots,x_n)=y}
\min_i\mu_{A_i}(x_i)
$$

Así se extiende una transformación definida sobre valores precisos a conceptos difusos.

Si varios valores de entrada producen el mismo $y$, el supremo conserva el mayor grado compatible con esa salida.

---

## Razonamiento difuso

El modus ponens generalizado combina un hecho parcial con una regla:

- Regla: si $x$ es $A$, entonces $y$ es $B$.
- Hecho: $x$ es $A'$.
- Conclusión: $y$ es $B'$.

La conclusión se obtiene componiendo $A'$ con la relación difusa que representa la regla. Su fuerza depende de la compatibilidad entre $A'$ y $A$.

<div class="callout">
La inferencia no consiste en buscar una regla idéntica: propaga grados de compatibilidad entre antecedentes y consecuentes.
</div>

---

## Reglas difusas

Una regla Mamdani adopta la forma:

> Si temperatura es alta **y** humedad es alta, entonces ventilación es intensa.

Para entradas $x_0,y_0$, la fuerza de activación con t-norma mínimo es:

$$
\alpha_i=\min\left(\mu_{A_i}(x_0),\mu_{B_i}(y_0)\right)
$$

El consecuente se recorta o escala según $\alpha_i$, dependiendo del método de implicación.

Así, una regla puede contribuir parcialmente. Varias reglas pueden activarse al mismo tiempo y aportar regiones distintas al conjunto de salida.

---

## Base de reglas

Una regla aislada representa conocimiento local. La base completa debe cubrir la región operacional.

| Temperatura | Humedad | Ventilación |
|---|---|---|
| baja | cualquiera | baja |
| confortable | baja | baja |
| confortable | alta | media |
| alta | baja | media |
| alta | alta | alta |

<div class="warn">
Reglas contradictorias son admisibles, pero su interacción debe ser deliberada y evaluada sobre todo el dominio.
</div>

---

## Sistema de inferencia difuso

![w:760](images/fuzzy_steps.png)

1. Fusificación de entradas numéricas.
2. Evaluación de antecedentes.
3. Implicación de cada regla.
4. Agregación de consecuentes.
5. Desfusificación de la salida.

La **fusificación** transforma mediciones precisas en grados de pertenencia. La **desfusificación** realiza la operación práctica inversa: obtiene una acción numérica desde el conjunto de salida agregado.

---

## Agregación

![w:720](images/aggregation_rules.png)

Si $\mu'_i(z)$ es el consecuente activado por la regla $i$, la agregación estándar usa:

$$
\mu_{salida}(z)=\max_i \mu'_i(z)
$$

La salida agregada conserva la contribución de todas las reglas activas antes de convertirla en un valor numérico.

Usar el máximo evita sumar dos veces el solapamiento, pero hace que en cada punto solo prevalezca la regla con mayor contribución.

---

## Desfusificación por centroide

![w:620](images/centroid.png)

Para un universo continuo:

$$
z^*=\frac{\int_Z z\,\mu_{salida}(z)\,dz}
{\int_Z \mu_{salida}(z)\,dz}
$$

En un universo discretizado:

$$
z^*=\frac{\sum_j z_j\mu_{salida}(z_j)}{\sum_j\mu_{salida}(z_j)}
$$

El centroide usa toda la forma de salida. Si el denominador es cero, ninguna regla produjo una salida y el sistema debe definir explícitamente cómo responder.

---

## Otros métodos de desfusificación

| Método | Idea | Efecto práctico |
|---|---|---|
| Centroide | Centro de masa | Usa toda la forma agregada |
| Bisector | Divide el área en dos | Sensible a distribución del área |
| Mean of maxima | Promedio de máximos | Ignora zonas no máximas |
| Centre of sums | Centro ponderado de áreas | Puede contar solapamientos varias veces |

El método forma parte del modelo y debe declararse al comparar resultados.

---

## Ejemplo conductor: aire acondicionado

Entradas posibles:

- error de temperatura $e=T_{real}-T_{objetivo}$.
- cambio del error $\Delta e$.
- humedad o punto de rocío.

Salida: nivel de potencia o velocidad del ventilador.

<div class="example-space">
Si $e$ es positivo grande y $\Delta e$ es positivo, la acción debe enfriar con intensidad. Si $e$ es cercano a cero, debe evitar oscilaciones.
</div>

---

## Del artículo al modelo

Los artículos complementarios en `../../Material/` presentan control difuso de:

- una lavadora: carga, suciedad y sensibilidad de la ropa.
- aire acondicionado: temperatura, humedad, punto de rocío y voltaje.

Preguntas de lectura:

1. ¿Qué variables son medibles y cuáles son lingüísticas?
2. ¿Cómo se obtuvieron las membresías y reglas?
3. ¿Contra qué controlador o línea base se compara?
4. ¿Qué evidencia respalda ahorro, estabilidad o confort?

---

## Ejemplo ejecutable

El notebook [`notebook/02_control_difuso.ipynb`](notebook/02_control_difuso.ipynb) construye un controlador Mamdani con:

- `numpy` para universos discretos.
- `scikit-fuzzy` para antecedentes, consecuentes y reglas.
- `matplotlib` para inspeccionar la superficie de control.

Flujo de validación:

1. Probar puntos nominales y extremos.
2. Barrer el dominio de entradas.
3. Buscar discontinuidades o regiones sin cobertura.
4. Interpretar la superficie antes de usar el controlador.

---

## Qué se debe validar

- Rango y unidades de cada universo.
- Cobertura de las funciones de pertenencia.
- Consistencia de la base de reglas.
- Monotonicidad donde el dominio la exija.
- Comportamiento en fronteras y entradas no nominales.
- Sensibilidad al método de desfusificación.

<div class="callout">
La interpretabilidad no es automática: un sistema difuso es explicable solo si sus variables, membresías y reglas tienen una semántica defendible.
</div>

---

## Síntesis

- Los grados de pertenencia representan conceptos graduales.
- Las operaciones y relaciones permiten combinar evidencia difusa.
- Las reglas conectan conocimiento lingüístico con una salida.
- Un sistema Mamdani fusifica, infiere, agrega y desfusifica.
- La superficie completa debe validarse, no solo ejemplos aislados.

<div class="bridge">
La próxima unidad cambia reglas lingüísticas por poblaciones de candidatos: desde inferencia difusa hacia búsqueda evolutiva.
</div>

---

## Referencias y material complementario

- Material original de IPD434: `02_SistemasDifusos.ipynb` y `02_SciKit_Fuzzy.ipynb`.
- L. A. Zadeh, “Fuzzy Sets”, *Information and Control*, 1965.
- T. J. Ross, *Fuzzy Logic with Engineering Applications*, 3.ª ed., Wiley, 2010.
- N. Wulandari y A. G. Abdullah, “Design and Simulation of Washing Machine using Fuzzy Logic Controller”, 2018. Disponible en `../../Material/`.
- S. M. Sobhy y W. M. Khedr, “Developing of Fuzzy Logic Controller for Air Condition System”, 2015. Disponible en `../../Material/`.
