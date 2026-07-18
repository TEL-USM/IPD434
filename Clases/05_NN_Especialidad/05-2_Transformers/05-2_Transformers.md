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

# IPD434
## Transformers y transferencia de aprendizaje
### Atención, preentrenamiento y uso responsable

Dr. Nicolás Gálvez Ramírez<br>
Dr. Patricio Olivares Roncagliolo

---

## Conexión con redes neuronales

Las RNN procesan secuencias manteniendo un estado recurrente. Los transformers modelan relaciones entre posiciones mediante atención.

$$
(x_1,\dots,x_n)\xrightarrow{\text{self-attention}}
(h_1,\dots,h_n)
$$

<div class="bridge">
El cambio central no es solo la arquitectura: el preentrenamiento permite reutilizar representaciones en tareas con menos datos etiquetados.
</div>

---

## Objetivos

1. **Explicar** tokenización, embeddings y codificación posicional.
2. **Calcular** atención escalada producto punto.
3. **Distinguir** encoder, decoder y encoder–decoder.
4. **Comparar** uso directo, extracción de características y ajuste fino (*fine-tuning*).
5. **Utilizar** una canalización (*pipeline*) de Hugging Face con modelo explícito.
6. **Evaluar** sesgos, dominio, costo y reproducibilidad.

---

## Ruta de la clase

1. Convertimos texto en tokens, vectores y posiciones.
2. La atención decide qué posiciones aportan información a cada representación.
3. Varias cabezas y una red prealimentada forman el bloque transformer.
4. La máscara de atención determina si el modelo codifica, genera o transforma secuencias.
5. El preentrenamiento permite transferir representaciones a otra tarea.
6. La evaluación comprueba dominio, trazabilidad, costo y sesgos.

---

## De texto a vectores

Un tokenizer transforma texto en identificadores:

$$
\text{texto}\rightarrow(t_1,t_2,\dots,t_n)
$$

Cada token obtiene un embedding:

$$
E\in\mathbb R^{|V|\times d},\qquad x_i=E[t_i]+p_i
$$

donde $p_i$ representa la posición. La tokenización afecta la longitud, el vocabulario y el tratamiento de palabras no vistas.

Las subpalabras (*subwords*) permiten representar una palabra desconocida mediante fragmentos conocidos. Los tokens especiales pueden marcar el inicio, la separación, el relleno o el fin de una secuencia.

---

## Atención escalada

Desde una matriz de entradas $X$ se calculan:

$$
Q=XW_Q,\qquad K=XW_K,\qquad V=XW_V
$$

$$
\operatorname{Attention}(Q,K,V)=
\operatorname{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right)V
$$

Cada posición combina valores de otras posiciones según similitud entre queries y keys.

El producto $QK^\top$ produce un puntaje para cada par de posiciones. *Softmax* normaliza cada fila y genera pesos que suman $1$.

---

## Multi-head attention

Cada cabeza aprende proyecciones distintas:

$$
head_i=\operatorname{Attention}(QW_i^Q,KW_i^K,VW_i^V)
$$

$$
\operatorname{MHA}(Q,K,V)=
\operatorname{Concat}(head_1,\dots,head_h)W^O
$$

Varias cabezas permiten representar relaciones complementarias, aunque no garantizan interpretabilidad causal.

Por ejemplo, una cabeza puede favorecer dependencias cercanas y otra relacionar términos distantes. Esa especialización puede surgir, pero no está impuesta de antemano.

---

## Bloque transformer

Un bloque combina:

1. atención multi-cabeza.
2. conexión residual y normalización.
3. red prealimentada (*feed-forward*) por posición.
4. nueva conexión residual y normalización.

$$
FFN(x)=W_2\,\phi(W_1x+b_1)+b_2
$$

La atención completa tiene costo cuadrático $O(n^2)$ respecto de la longitud de secuencia.

La red prealimentada aplica la misma transformación a cada posición. La atención es el componente que intercambia información entre posiciones.

---

## Familias de arquitectura

| Tipo | Atención | Tareas típicas |
|---|---|---|
| Encoder | Bidireccional | Clasificación, embeddings, extracción |
| Decoder | Causal | Generación autoregresiva |
| Encoder–decoder | Entrada completa y salida causal | Traducción, resumen condicionado |

La arquitectura debe corresponder al patrón de entrada y salida, no solo al tamaño del modelo.

La atención causal impide que una posición utilice tokens futuros durante generación. Un encoder bidireccional sí puede usar contexto a ambos lados.

---

## Preentrenamiento y transferencia

El preentrenamiento aprende parámetros $\theta_0$ desde un corpus grande. Una tarea destino ajusta:

$$
\theta^*=\arg\min_\theta L_{destino}(\theta;\mathcal D_{destino})
\quad\text{inicializando en }\theta_0
$$

Opciones:

- inferencia directa.
- embeddings congelados.
- ajuste fino parcial.
- ajuste fino completo.

Cuantos más parámetros se ajustan, mayor es la capacidad de adaptación, pero también aumentan el costo y el riesgo de sobreajuste o pérdida de conocimiento previo.

---

## Transferencia más allá del texto

![w:570](Images/catdog.gif)

El principio también se aplica en visión: un modelo preentrenado aporta representaciones generales y se adapta una cabeza o parte de la red.

<div class="callout">
Transferir funciona cuando existe suficiente relación entre dominio fuente y destino. Esa relación debe comprobarse con validación propia.
</div>

---

## Canalización de Hugging Face

```python
from transformers import pipeline

classifier = pipeline(
    "text-classification",
    model="distilbert/distilbert-base-uncased-finetuned-sst-2-english",
)
classifier(["The service works well.", "The service failed again."])
```

Una `pipeline` encapsula tokenizador, modelo y posprocesamiento. Es apropiada para inferencia y prototipos, pero no reemplaza el protocolo experimental.

El resultado incluye una etiqueta y un puntaje del modelo. Ese puntaje no debe interpretarse automáticamente como una probabilidad calibrada para decisiones reales.

---

## Modelo explícito y trazabilidad

Siempre registrar:

- identificador del modelo.
- revisión o identificador de cambio cuando se requiere reproducibilidad estricta.
- versión de `transformers` y backend.
- tarea, tokenizer y parámetros.
- hardware y precisión numérica.
- fecha y licencia del artefacto.

<div class="warn">
Usar el modelo predeterminado de una `pipeline` puede cambiar resultados entre entornos o versiones.
</div>

---

## Ajuste fino para clasificación

Flujo mínimo:

1. Definir etiquetas y criterio de inclusión.
2. Separar datos por entidad, tiempo o contexto relevante.
3. Tokenizar con truncamiento y padding controlados.
4. Entrenar con validación y detención.
5. Evaluar por clase y segmento.
6. Comparar contra una línea base no neuronal.

La clase mayoritaria y un modelo lineal sobre TF-IDF son líneas base útiles.

La partición debe evitar que textos casi duplicados, autores o entidades aparezcan simultáneamente en entrenamiento y prueba, pues ello puede inflar el desempeño.

---

## Riesgos de evaluación

- Los datos de preentrenamiento pueden solaparse con conjuntos de referencia.
- Un puntaje agregado oculta grupos con peor desempeño.
- Cambios de idioma o dominio degradan resultados.
- Probabilidades de softmax pueden estar mal calibradas.
- Las instrucciones de entrada y el truncamiento alteran la distribución efectiva.

$$
\text{desempe\~no fuera de dominio}\ne
\text{desempe\~no en una partici\'on aleatoria}
$$

Por ello conviene analizar errores por idioma, longitud, clase y subdominio, además de reportar una métrica agregada.

---

## Ejemplo ejecutable

[`notebooks/05_2_pipeline_transformers.ipynb`](notebooks/05_2_pipeline_transformers.ipynb):

- carga un modelo de clasificación explícito.
- procesa ejemplos en lote.
- inspecciona tokenización y truncamiento.
- contrasta frases ambiguas y cambio de dominio.
- guarda resultados tabulares, no el modelo descargado.

El modelo se descarga desde Hugging Face al ejecutar por primera vez.

---

## Síntesis

- Self-attention combina información entre posiciones.
- Multi-head attention aprende relaciones complementarias.
- El preentrenamiento permite transferir representaciones.
- `pipeline` facilita la inferencia, pero se deben fijar el modelo y el entorno.
- La validez depende del dominio, datos, métricas y análisis de errores.

<div class="bridge">
La siguiente especialidad estudia aprendizaje de representaciones y generación mediante autoencoders, VAE y GAN.
</div>

---

## Referencias

- Material original: notebooks de `05-2_Transformers/`.
- A. Vaswani et al., “Attention Is All You Need”, 2017.
- J. Devlin et al., “BERT: Pre-training of Deep Bidirectional Transformers”, 2019.
- Hugging Face Transformers, documentación oficial de pipelines.
- TensorFlow/Keras, documentación oficial de transferencia de aprendizaje.
