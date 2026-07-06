# Presentaciones IPD434

Las presentaciones están escritas en Markdown compatible con Marp, usan MathJax para fórmulas LaTeX y reutilizan los recursos gráficos de cada unidad. Los notebooks originales se conservan como material fuente; los ejemplos nuevos están en la subcarpeta `notebooks/` de la unidad correspondiente.

## Secuencia principal

1. [Presentación del curso](00_Introduccion/00_Introduccion.md)
2. [Introducción a Soft Computing](01_IntroduccionSoftComputing/01_IntroduccionSoftComputing.md)
3. [Sistemas difusos](02_SistemasDifusos/02_SistemasDifusos.md)
4. [Computación evolutiva](03_ComputacionEvolutiva/03_ComputacionEvolutiva.md)
5. [Redes neuronales](04_RedesNeuronales/04_RedesNeuronales.md)

## Especialidades

- [Computación evolutiva avanzada](05_EA_Especialidad/05_EA_Avanzado.md)
- [Aprendizaje por refuerzo](05_NN_Especialidad/05-1_DeepReninforcementLearning/05-1_ReinforcementLearning.md)
- [Transformers y transferencia](05_NN_Especialidad/05-2_Transformers/05-2_Transformers.md)
- [Autoencoders, VAE y GAN](05_NN_Especialidad/05-3_GAN_AE/05-3_GAN_AE.md)
- [Arquitecturas CNN avanzadas](05_NN_Especialidad/05-4_AdvancedCNNArchitectures/05-4_AdvancedCNNArchitectures.md)

## Notebooks nuevos

| Unidad | Ejemplo |
|---|---|
| Sistemas difusos | [Control difuso](02_SistemasDifusos/notebooks/02_control_difuso.ipynb) |
| Computación evolutiva | [GA para N reinas](03_ComputacionEvolutiva/notebooks/03_ga_n_reinas.ipynb) |
| Redes neuronales | [Fashion-MNIST](04_RedesNeuronales/notebooks/04_clasificacion_fashion_mnist.ipynb) |
| Evolutiva avanzada | [ACO para TSP](05_EA_Especialidad/notebooks/05_aco_tsp.ipynb) |
| Aprendizaje por refuerzo | [Q-learning](05_NN_Especialidad/05-1_DeepReninforcementLearning/notebooks/05_1_q_learning.ipynb) |
| Transformers | [Pipeline de clasificación](05_NN_Especialidad/05-2_Transformers/notebooks/05_2_pipeline_transformers.ipynb) |
| Modelos generativos | [Autoencoder convolucional](05_NN_Especialidad/05-3_GAN_AE/notebooks/05_3_autoencoder_convolucional.ipynb) |
| CNN avanzadas | [Bloque residual](05_NN_Especialidad/05-4_AdvancedCNNArchitectures/notebooks/05_4_bloque_residual.ipynb) |

Cada notebook declara sus dependencias en una celda inicial. Los conjuntos de datos o pesos externos se descargan únicamente al ejecutar la celda correspondiente.
