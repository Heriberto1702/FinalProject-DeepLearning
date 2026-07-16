# FinalProject-DeepLearning
Este github contien todos los archivos del proyecto final de la asignatura de deeplearning.

Este proyecto compara dos enfoques de Deep Learning para clasificar radiografías de tórax en **NORMAL** o **PNEUMONIA**:

1. Una **CNN entrenada desde cero** (baseline).
2. **Transfer Learning** con **ResNet18** preentrenada en ImageNet.

Ambos modelos se entrenan y evalúan sobre el mismo dataset, precedidos por un Análisis Exploratorio de Datos (EDA) que justifica las decisiones de preprocesamiento tomadas en los dos notebooks de modelado.

## 📁 Estructura del repositorio

```
.
├── 03_EDA_Pneumonia.ipynb                       # Paso 1: análisis exploratorio del dataset
├── 01_cnn_baseline_pneumonia_mejorado.ipynb      # Paso 2: CNN entrenada desde cero
├── 02_Transfer_Learning_Pneumonia_mejorado.ipynb # Paso 3: Transfer Learning con ResNet18
├── images_sample/                                # (se crea automáticamente) imágenes para probar predicción individual
└── data/raw/                                     # (se crea automáticamente) dataset descargado desde Kaggle
```

## Guía de ejecución paso a paso

Se recomienda ejecutar los notebooks **en este orden**, ya que el EDA justifica las decisiones tomadas en los dos modelos:

### Paso 1 — Análisis Exploratorio de Datos
**Notebook:** `03_EDA_Pneumonia.ipynb`

1. Abrir el notebook y ejecutar todas las celdas en orden (`Kernel → Restart & Run All`).
2. La primera celda de código instalará dependencias faltantes y descargará el dataset desde Kaggle automáticamente (puede tardar unos minutos la primera vez).
3. Revisar los gráficos generados: distribución de clases por conjunto, ejemplos visuales de radiografías, distribución de dimensiones de imagen, y distribución de intensidad de píxeles.
4. Leer la sección final **"Conclusiones del EDA"**, que resume el desbalance de clases y las decisiones de preprocesamiento recomendadas para los siguientes notebooks.

No se requiere GPU para este notebook; corre rápido incluso en CPU.

### Paso 2 — CNN Baseline (desde cero)
**Notebook:** `01_cnn_baseline_pneumonia_mejorado.ipynb`

1. Ejecutar todas las celdas en orden (`Kernel → Restart & Run All`).
2. El notebook descarga el dataset (si no se descargó ya en el paso 1, usa la misma carpeta `data/raw`), lo redimensiona a 128×128 en escala de grises, y re-particiona el conjunto de entrenamiento en 80% train / 20% validación.
3. La celda de entrenamiento (sección **5️⃣ Entrenamiento**) corre 15–20 épocas e imprime el accuracy de entrenamiento y validación por época. En CPU puede tardar entre 20 y 40 minutos; en GPU, unos pocos minutos.
4. La celda siguiente (**Gráfico de las épocas**) grafica las curvas de pérdida y accuracy — debe ejecutarse **después** de la celda de entrenamiento, ya que depende de sus resultados.
5. La sección **6️⃣ Evaluación en Conjunto de Prueba** reporta el accuracy final sobre el conjunto `test` oficial (no visto durante entrenamiento).
6. (Opcional) La última sección clasifica imágenes individuales. Para probarla, colocar archivos `.jpg/.jpeg/.png` dentro de la carpeta `images_sample/` (se crea automáticamente en la raíz del proyecto) y volver a ejecutar esa celda.

### Paso 3 — Transfer Learning (ResNet18)
**Notebook:** `02_Transfer_Learning_Pneumonia_mejorado.ipynb`

1. Ejecutar todas las celdas en orden (`Kernel → Restart & Run All`).
2. Este notebook descarga automáticamente los pesos preentrenados de ResNet18 (requiere internet la primera vez).
3. Las imágenes se procesan a color y se redimensionan a 224×224 (tamaño requerido por ResNet18).
4. El entrenamiento ocurre en **dos fases**, cada una en su propia celda:
   - **Fase 1 — Feature Extraction** (5 épocas): solo se entrena la última capa; las demás quedan congeladas.
   - **Fase 2 — Fine-tuning** (3 épocas): se descongelan las últimas capas (`layer4` y la capa final) y se ajustan con una tasa de aprendizaje muy baja.
5. La sección de evaluación reporta el accuracy sobre el conjunto `test` oficial, igual que en el baseline — esto permite comparar ambos modelos de forma directa.
6. (Opcional) Igual que en el baseline, se pueden clasificar imágenes propias colocándolas en `images_sample/`.


## Resultados esperados (referencia)

| Modelo | Accuracy validación | Accuracy test |
|---|---|---|
| CNN Baseline (desde cero) | ~97.5% | ~78.2% |
| Transfer Learning (ResNet18) | ~98.1% | ~81.4% |

Y eso seria todo.

Gracias de antemano Freddy!
