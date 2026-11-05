# Guía docente: arquitecturas CNN avanzadas

Esta guía complementa `05-4_AdvancedCNNArchitectures.md`. Organiza ResNet, Inception y EfficientNet como respuestas a problemas concretos de profundidad, escala y costo.

## Tesis de la clase

Aumentar profundidad, ancho o resolución puede mejorar capacidad, pero también dificulta optimización y aumenta costo. Las arquitecturas avanzadas introducen sesgos de diseño que permiten usar esos recursos con mayor eficacia.

## Profundidad y campo receptivo

Las primeras capas suelen aprender patrones locales. Capas posteriores combinan esos patrones en estructuras de mayor escala.

El campo receptivo teórico crece con tamaño de kernel, paso y profundidad. El campo receptivo efectivo suele concentrar mayor influencia cerca del centro.

Dos convoluciones $3\times3$ producen un campo receptivo de $5\times5$ con no linealidad intermedia y, a menudo, menos parámetros que una convolución $5\times5$ densa.

## Gradientes que se desvanecen o explotan

En una composición profunda, el gradiente contiene productos de Jacobianos:

$$
\frac{\partial L}{\partial x_0}=
\frac{\partial L}{\partial x_L}
\prod_{l=1}^{L}\frac{\partial x_l}{\partial x_{l-1}}
$$

Si las normas son repetidamente menores que uno, el gradiente se desvanece. Si son mayores, puede explotar.

Mitigaciones:

- inicialización apropiada
- activaciones como ReLU
- normalización
- conexiones residuales
- recorte del gradiente cuando corresponde

La degradación de entrenamiento es distinta del sobreajuste. Una red más profunda puede tener mayor error incluso en entrenamiento porque resulta más difícil de optimizar.

## Bloque residual

$$
y=F(x,W)+x
$$

En lugar de aprender $H(x)$ directamente, el bloque aprende:

$$
F(x)=H(x)-x
$$

Si la transformación adicional no ayuda, $F(x)$ puede aproximar cero y el bloque se acerca a la identidad.

La ruta directa facilita flujo de información y gradientes:

$$
\frac{\partial y}{\partial x}=\frac{\partial F}{\partial x}+I
$$

## Cambio de dimensiones

La suma exige formas compatibles. Cuando cambian canales o resolución:

$$
y=F(x,W)+W_sx
$$

$W_s$ puede ser una convolución $1\times1$ con paso mayor que uno. La rama principal debe realizar una reducción compatible.

No se deben forzar dimensiones mediante operaciones arbitrarias porque eso cambia la arquitectura y su costo.

## ResNet

ResNet apila bloques residuales y aumenta canales cuando reduce resolución. Existen bloques básicos y bloques cuello de botella.

Un cuello de botella típico usa $1\times1$, $3\times3$ y $1\times1$. La primera proyección reduce canales, la convolución procesa espacio y la última restaura canales.

ResNet se usa como red base para clasificación, detección y segmentación.

## Módulo Inception

Procesa la entrada mediante ramas paralelas:

- convolución $1\times1$
- convolución $3\times3$
- convolución $5\times5$ o factorizada
- agrupación seguida de proyección

Las salidas se concatenan por canales. Las ramas capturan escalas distintas.

Las proyecciones $1\times1$ reducen canales antes de convoluciones costosas.

## Convolución $1\times1$

Para cada posición espacial combina todos los canales:

$$
y_{h,w,o}=\sum_cW_{o,c}x_{h,w,c}+b_o
$$

No mezcla vecinos espaciales, pero aprende nuevas combinaciones de características y cambia el número de canales.

## EfficientNet y escalamiento compuesto

En vez de escalar una sola dimensión, EfficientNet coordina:

$$
d=\alpha^\phi,\qquad w=\beta^\phi,\qquad r=\gamma^\phi
$$

donde $d$ es profundidad, $w$ ancho y $r$ resolución.

Una restricción aproximada de costo es:

$$
\alpha\beta^2\gamma^2\approx2
$$

La fórmula expresa que ancho y resolución afectan el costo de manera aproximadamente cuadrática. Los coeficientes se determinan para una familia base.

## Bloque MBConv

Un bloque móvil invertido contiene:

1. expansión $1\times1$
2. convolución por canal
3. atención de canales *squeeze-and-excitation*
4. proyección $1\times1$
5. atajo cuando forma y paso lo permiten

La convolución *depthwise* filtra cada canal por separado. La proyección puntual combina canales. Esto reduce operaciones respecto de una convolución densa.

## Normalización por lotes

Para un mini-lote:

$$
\hat x=\frac{x-\mu_B}{\sqrt{\sigma_B^2+\epsilon}}
$$

$$
y=\gamma\hat x+\beta
$$

Durante entrenamiento usa estadísticas del lote. Durante inferencia usa promedios acumulados. Lotes muy pequeños pueden producir estadísticas ruidosas.

Debe respetarse el modo entrenamiento o inferencia al ajustar parcialmente un modelo preentrenado.

## Aumento de datos

Las transformaciones deben conservar la etiqueta en el dominio. Pueden incluir recorte, traslación, rotación limitada, cambios fotométricos y enmascaramiento.

Voltear una imagen puede ser válido para objetos naturales y errado para texto, lateralidad médica o señales direccionales.

El aumento pertenece al conjunto de entrenamiento. No se debe aplicar aleatoriamente a prueba salvo que se use una estrategia declarada de *test-time augmentation*.

## Mixup

$$
\tilde x=\lambda x_i+(1-\lambda)x_j
$$

$$
\tilde y=\lambda y_i+(1-\lambda)y_j
$$

con $\lambda$ muestreado de una distribución beta. Mixup suaviza transiciones entre clases y modifica tanto entradas como etiquetas.

## Suavizado de etiquetas

Para $K$ clases, una forma es:

$$
y_k^{LS}=(1-\varepsilon)y_k+\frac{\varepsilon}{K}
$$

Evita objetivos exactamente cero o uno. Puede mejorar generalización, pero cambia confianza y debe evaluarse junto con calibración.

## Transferencia de aprendizaje

1. cargar pesos preentrenados
2. aplicar el preprocesamiento correcto
3. reemplazar la cabeza
4. congelar la base al inicio
5. entrenar la cabeza
6. descongelar parcialmente con tasa menor
7. evaluar cambio de dominio

Preprocesamientos distintos entre familias pueden cambiar el resultado de forma severa.

## Comparación de arquitecturas

No basta con exactitud. Conviene reportar:

- número de parámetros
- operaciones o latencia medida
- memoria
- tamaño de entrada
- energía cuando sea pertinente
- desempeño por clase
- costo de entrenamiento

Las mediciones deben usar el mismo hardware, tamaño de lote, precisión y canal de entrada.

## Confusiones frecuentes

- una red más profunda no siempre entrena mejor
- residual no significa que se omita la transformación
- convolución $1\times1$ sí aprende relaciones entre canales
- menos parámetros no garantiza menor latencia
- aumento de datos no corrige un sesgo de muestreo
- pesos preentrenados no eliminan el cambio de dominio

## Preparación y preguntas

- Dibujar un bloque residual con y sin proyección.
- Calcular formas de tensores en un cambio de resolución.
- Comparar costo de convolución densa y *depthwise separable*.
- Proponer un aumento inválido para un dominio concreto.
- Revisar en el notebook que ambas redes tengan un presupuesto comparable.

