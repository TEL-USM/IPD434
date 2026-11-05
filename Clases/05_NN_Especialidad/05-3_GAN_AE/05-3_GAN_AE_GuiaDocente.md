# Guía docente: autoencoders, VAE y GAN

Esta guía desarrolla `05-3_GAN_AE.md`. El hilo común es el aprendizaje de representaciones latentes y distribuciones de datos.

## Tesis de la clase

Un autoencoder aprende a reconstruir, un VAE organiza probabilísticamente el espacio latente y una GAN aprende mediante un juego adversarial. Sus objetivos y criterios de evaluación son distintos.

## Autoencoder

$$
z=f_\theta(x),\qquad \hat x=g_\phi(z)
$$

El entrenamiento minimiza:

$$
(\theta^*,\phi^*)=\arg\min_{\theta,\phi}
\frac{1}{n}\sum_i\ell(x_i,g_\phi(f_\theta(x_i)))
$$

El encoder produce $z$. El decoder reconstruye $\hat x$. Si la red tiene capacidad suficiente y ninguna restricción, puede aprender una copia trivial.

## Cuello de botella

Puede imponerse mediante:

- menor dimensión latente
- ruido
- penalización de activaciones
- dispersidad
- regularización contractual
- arquitectura convolucional

La restricción define qué información resulta más barato conservar.

## Pérdida de reconstrucción

MSE supone que errores cuadráticos de píxeles son relevantes. Entropía cruzada binaria puede usarse con salidas en $[0,1]$, aunque su interpretación depende del modelo de observación.

Una pérdida perceptual compara características de otra red y puede alinearse mejor con apariencia. También introduce sesgos de esa red.

## Aplicaciones

### Reducción de dimensión

$z$ puede actuar como representación compacta. A diferencia de PCA, el mapeo puede ser no lineal.

### Detección de anomalías

Se entrena con datos normales y se usa error de reconstrucción. Un umbral debe calibrarse en validación con ejemplos representativos.

Error alto no implica anomalía de forma universal. Puede indicar ruido, cambio de sensor o una región normal poco representada.

### Eliminación de ruido

Se entrena con entrada corrupta $\tilde x$ y objetivo limpio $x$:

$$
\tilde x\rightarrow f_\theta\rightarrow z\rightarrow g_\phi\rightarrow\hat x\approx x
$$

El ruido de entrenamiento debe parecerse a perturbaciones reales.

## Autoencoder convolucional

El encoder reduce resolución y aumenta canales. El decoder recupera resolución mediante redimensionamiento, convoluciones transpuestas u otras operaciones.

La forma de salida debe coincidir con la entrada. La activación final debe ser coherente con la normalización.

Las convoluciones transpuestas pueden producir artefactos de tablero. Redimensionar seguido de convolución es una alternativa.

## Limitación generativa del AE determinista

Un AE puede reconstruir bien puntos observados, pero dejar huecos en el espacio latente. Muestrear un $z$ arbitrario puede producir una salida sin sentido.

La reconstrucción no obliga a que regiones vecinas de $z$ correspondan a muestras plausibles.

## Variational Autoencoder

El encoder produce parámetros de una distribución:

$$
q_\phi(z\mid x)=\mathcal N(\mu(x),\operatorname{diag}(\sigma^2(x)))
$$

Se usa un prior, típicamente:

$$
p(z)=\mathcal N(0,I)
$$

El objetivo minimizado puede escribirse como:

$$
L_{VAE}=L_{rec}+\beta D_{KL}\bigl(q_\phi(z\mid x)\|p(z)\bigr)
$$

La reconstrucción conserva información del dato. El término KL aproxima las distribuciones latentes al prior y regulariza el espacio.

$\beta$ controla el compromiso. Un KL demasiado fuerte puede producir colapso del posterior, donde el decoder ignora $z$.

## Reparametrización

$$
\epsilon\sim\mathcal N(0,I)
$$

$$
z=\mu+\sigma\odot\epsilon
$$

La aleatoriedad queda en $\epsilon$. $z$ se vuelve una función diferenciable de $\mu$ y $\sigma$, lo que permite retropropagación.

## GAN

Una GAN contiene generador $G$ y discriminador $D$:

$$
\min_G\max_D
\mathbb E_{x\sim p_{data}}[\log D(x)]
+\mathbb E_{z\sim p(z)}[\log(1-D(G(z)))]
$$

$D$ distingue datos reales de generados. $G$ produce muestras para engañar a $D$.

En la práctica, el generador suele minimizar $-\log D(G(z))$ para recibir gradientes más fuertes al inicio.

## Entrenamiento adversarial

1. muestrear datos reales
2. muestrear ruido latente
3. generar muestras falsas
4. actualizar discriminador
5. actualizar generador manteniendo fijo el discriminador

El equilibrio es dinámico. Si $D$ domina, $G$ puede recibir poca señal. Si $D$ es débil, la señal tampoco orienta calidad.

## Dificultades de GAN

- inestabilidad y oscilaciones
- colapso de modos
- gradientes débiles
- alta sensibilidad a arquitectura y tasa de aprendizaje
- dificultad para comparar ejecuciones

El colapso de modos ocurre cuando entradas latentes distintas producen pocas variedades de salida.

Wasserstein GAN y penalización de gradiente modifican el objetivo para mejorar la señal, pero no eliminan la necesidad de evaluar diversidad.

## Evaluación

Para reconstrucción:

- pérdida en conjunto independiente
- distribución de errores
- evaluación por subgrupo
- calidad de la tarea posterior

Para generación:

- fidelidad
- diversidad
- cobertura de modos
- memorizar versus generar
- evaluación del dominio

FID compara media y covarianza de características de datos reales y generados. Depende de la red de características, tamaño de muestra y dominio. No reemplaza inspección ni evaluación específica.

## Confusiones frecuentes

- reconstruir no equivale a generar bien
- un espacio latente de dos dimensiones no garantiza interpretabilidad
- VAE no es un AE con ruido agregado sin objetivo probabilístico
- discriminador perfecto no significa entrenamiento exitoso
- imágenes visualmente buenas no demuestran diversidad
- error de reconstrucción no es una etiqueta de anomalía

## Preparación y preguntas

- Mostrar un AE con capacidad suficiente para copiar.
- Explicar por qué el prior facilita muestreo en VAE.
- Derivar el papel de $\epsilon$ en reparametrización.
- Dibujar el ciclo alternado de GAN.
- Revisar que el notebook separe entrenamiento y calibración de umbral.

