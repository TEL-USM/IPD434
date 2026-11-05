---
marp: true
math: katex
paginate: true
style: |
  section { font-size: 27px }
  section { line-height: 1.35 }
  section.lead { text-align: center }
  section.lead h1 { font-size: 2.1em }
  h2, h3 { color: #12394f }
  code { font-size: 0.84em }
  table { font-size: 0.77em }
  img { max-height: 430px }
  img { display: block }
  img { margin: 0 auto }
  .columns { display: grid }
  .columns { grid-template-columns: 1fr 1fr }
  .columns { gap: 32px }
  .columns { align-items: start }
  .callout { background: #eef6fb }
  .callout { border-left: 6px solid #2f6f9f }
  .callout { border-radius: 6px }
  .callout { padding: 0.7em 0.9em }
  .bridge { background: #f7f8fa }
  .bridge { border-left: 6px solid #6b7280 }
  .bridge { border-radius: 6px }
  .bridge { padding: 0.65em 0.9em }
  .warn { background: #fff4df }
  .warn { border-left: 6px solid #b7791f }
  .warn { border-radius: 6px }
  .warn { padding: 0.65em 0.9em }
  .example-space { background: #eef8f1 }
  .example-space { border-left: 6px solid #2f855a }
  .example-space { border-radius: 6px }
  .example-space { padding: 0.65em 0.9em }
  .small { font-size: 0.82em }
  .diagram-focus { width: 650px }
  .diagram-focus { height: 520px }
  .diagram-focus { margin: 0 auto }
  .diagram-focus { overflow: hidden }
  .diagram-focus { position: relative }
  .diagram-focus img { position: absolute }
  .diagram-focus img { width: auto }
  .diagram-focus img { height: 580px }
  .diagram-focus img { max-width: none }
  .diagram-focus img { max-height: none }
  .diagram-focus img { left: -106px }
  .diagram-focus img { top: -16px }
  .diagram-focus img { margin: 0 }
  .matrix-focus { text-align: center }
  .matrix-focus img { display: block }
  .matrix-focus img { width: 900px }
  .matrix-focus img { max-width: 100% }
  .matrix-focus img { height: auto }
  .matrix-focus img { max-height: none }
  .matrix-focus img { margin: 28px auto 0 }
  .activation-grid { display: grid }
  .activation-grid { grid-template-columns: repeat(3, 1fr) }
  .activation-grid { gap: 8px 14px }
  .activation-grid { align-items: start }
  .activation-grid img { width: 100% }
  .activation-grid img { max-height: none }
  .activation-grid img { height: auto }
  .digit-example { display: inline-block }
  .digit-example { vertical-align: middle }
  .digit-example { margin-right: 8px }
  .code-compare pre { font-size: 0.52em }
  .code-compare pre { line-height: 1.08 }
  .shared-code pre { font-size: 0.58em }
  .shared-code pre { line-height: 1.1 }
---
<!-- _class: lead -->

![w:125](images/utfsm.png)

# IPD434
## Redes generativas
### Autoencoders, VAE y GAN

Dr. Patricio Olivares Roncagliolo

Ingeniería Civil Telemática · Universidad Técnica Federico Santa María

---

## Autoencoder: comprimir y reconstruir

- Un **encoder** transforma la entrada $x$ en una representación latente $z$.
- Un **decoder** reconstruye la entrada como $\hat{x}$.
- El entrenamiento busca que la reconstrucción se parezca a la entrada.

$$x\longrightarrow z\longrightarrow\hat{x}$$

<img src="images/AE.png" alt="Encoder y decoder de un autoencoder" width="700px"/>

---

## Una representación latente compacta

- El encoder reduce la dimensión y conserva rasgos útiles.
- El decoder aprende a recuperar la estructura original desde $z$.
- Una reconstrucción más cercana indica que la representación retiene información relevante.

<img src="images/AE2.png" alt="Representación latente de un autoencoder" width="650px"/>

---

## ¿Dónde se usan los autoencoders?

- **Reducción de dimensionalidad:** representar datos con menos variables.
- **Eliminación de ruido:** reconstruir la señal relevante de una entrada perturbada.
- **Detección de anomalías:** buscar ejemplos que el modelo reconstruye mal.

<img src="images/AE_noise.png" alt="Ejemplo de eliminación de ruido" width="760px"/>

---

## Autoencoder convolucional

- Las convoluciones preservan la estructura espacial de las imágenes.
- El encoder reduce resolución y extrae patrones locales.
- El decoder reconstruye la imagen desde las características comprimidas.

[Notebook PyTorch: autoencoders convolucionales y VAE](notebook/05_3_autoencoders_vae_pytorch.ipynb)

---

## VAE: una distribución en el espacio latente

- Un **Variational Autoencoder** aprende una distribución latente, no solo un código fijo.
- El encoder estima parámetros como la media $\mu$ y la varianza $\sigma^2$.
- Muestrear desde esa distribución permite generar ejemplos nuevos.

<img src="images/VAE_Basic.png" alt="Estructura de un VAE" width="700px"/>

---

## Muestrear y generar con un VAE

La reparametrización expresa la muestra latente de forma diferenciable:

$$z=\mu+\sigma\odot\epsilon,\qquad \epsilon\sim\mathcal{N}(0,I)$$

- El decoder convierte cada muestra $z$ en una observación.
- Cambiar la muestra permite producir salidas distintas.

<img src="images/VAE2.png" alt="Muestreo de variables latentes en un VAE" width="700px"/>

---

## Práctica: autoencoders y VAE en PyTorch

El notebook compara reconstrucciones de modelos densos y convolucionales, entrena un VAE y muestra muestras latentes.

[Abrir notebook PyTorch de autoencoders y VAE](notebook/05_3_autoencoders_vae_pytorch.ipynb)

---

## GAN: dos redes con objetivos opuestos

- El **generador** transforma ruido $z$ en datos sintéticos $G(z)$.
- El **discriminador** estima si una entrada proviene de datos reales o fue generada.
- Ambos se entrenan en competencia.

<img src="images/GAN.png" alt="Generador y discriminador de una GAN" width="760px"/>

---

## El entrenamiento alterna dos fases

<div class="columns">
<div>
<h3>1. Discriminador</h3>
<ul>
<li>Ve ejemplos reales y generados.</li>
<li>Aprende a distinguir ambas clases.</li>
</ul>
</div>
<div>
<h3>2. Generador</h3>
<ul>
<li>Recibe ruido aleatorio.</li>
<li>Aprende a engañar al discriminador.</li>
</ul>
</div>
</div>

---

## Fase 1: el discriminador distingue

- Se mezclan muestras reales $x$ con muestras falsas $G(z)$.
- Las etiquetas objetivo son 1 para reales y 0 para generadas.
- Su objetivo es maximizar:

$$\log D(x)+\log\left(1-D(G(z))\right)$$

---

## Fase 2: el generador engaña

- Se genera un nuevo lote a partir de ruido.
- El discriminador recibe las imágenes generadas, pero se busca que responda “real”.
- El generador aumenta la probabilidad $D(G(z))$:

$$\max_G\;\log D(G(z))$$

---

## Una dinámica adversarial inestable

- Generador y discriminador se actualizan alternadamente.
- El objetivo ideal es que el discriminador no distinga mejor que al azar (50 %).
- Si uno de los modelos se vuelve demasiado fuerte, el otro puede dejar de aprender.

---

## De ruido a imagen: el generador antes de aprender

- Una red puede transformar un vector latente en una imagen de tamaño $28\times28$.
- Antes del entrenamiento, sus pesos aleatorios producen patrones sin significado.
- El ejemplo prepara la intuición para el entrenamiento adversarial.

[Ver el generador y el entrenamiento GAN en el notebook PyTorch de MNIST](notebook/05_3_gan_mnist_pytorch.ipynb)

---

## Ejemplo completo: GAN para MNIST

- El generador convierte ruido en imágenes de dígitos.
- El discriminador clasifica imágenes reales y generadas.
- Al terminar cada época se revisan muestras nuevas y las pérdidas de ambos modelos.

[Abrir notebook PyTorch de GAN con MNIST](notebook/05_3_gan_mnist_pytorch.ipynb)

---

## Autoencoder, VAE y GAN: tres objetivos distintos

| Modelo | Qué aprende | Para qué sirve |
|---|---|---|
| AE | Una representación que reconstruye la entrada | Compresión y reconstrucción |
| VAE | Una distribución latente muestreable | Generar variaciones plausibles |
| GAN | Un generador que engaña a un discriminador | Crear datos sintéticos realistas |

---

## Ideas clave

- AE y VAE aprenden a reconstruir; el VAE además organiza el espacio latente para muestrear.
- Una GAN aprende mediante la competencia entre generador y discriminador.
- El equilibrio y la calidad de las muestras dependen del entrenamiento, no solo de la arquitectura.

---

## Lecturas y fuentes de figuras

- A. Géron, *Hands-on Machine Learning with Scikit-Learn, Keras & TensorFlow*.
- Diagrama de AE: [Applied Deep Learning](https://towardsdatascience.com/applied-deep-learning-part-3-autoencoders-1c083af4d798).
- VAE: [Wikipedia](https://en.wikipedia.org/wiki/Variational_autoencoder) y [Towards Data Science](https://towardsdatascience.com/intuitively-understanding-variational-autoencoders-1bfe67eb5daf).
- GAN: [Microsoft Research](https://www.microsoft.com/en-us/research/blog/how-can-generative-adversarial-networks-learn-real-life-distributions-easily/).
