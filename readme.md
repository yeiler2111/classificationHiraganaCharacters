# Trabajo Final ML — Clasificación de caracteres japoneses

## Descripción
- Objetivo: Clasificar imágenes 28x28 de 10 caracteres hiragana.
- Notebook principal: caracteresjaponeses/TrabajoFinalML.ipynb
- Dataset esperado: caracteresjaponeses/test_data.csv (10,000 filas; columnas: Unnamed: 0, label, pixeles 0..783).

## Requisitos
- SO: Linux
- Python: 3.12
- Entorno virtual: .venv/ en la raíz del repo

## Configuración del entorno
- [1. Activar entorno virtual]
bash
cd /home/kurodev/Documentos/unimag/ia
source .venv/bin/activate


- [2. Instalar dependencias del proyecto]
bash
python -m pip install --upgrade pip
pip install -r requirements.txt


- [3. Instalar librerías usadas en el notebook]
Estas librerías pueden no estar aún en requirements.txt y son necesarias:
bash
pip install scikit-learn scipy plotly nbformat seaborn


- [4. (Opcional) Registrar kernel de Jupyter]
bash
python -m ipykernel install --user --name ia-venv --display-name "Python (ia-venv)"


## Ejecución del notebook
- [A. Lanzar Jupyter]
bash
jupyter notebook
# o
jupyter lab


- [B. Abrir] caracteresjaponeses/TrabajoFinalML.ipynb
- [C. Seleccionar kernel] "Python (ia-venv)" si aplica.
- [D. Verificar el CSV] Asegúrate de que test_data.csv esté en caracteresjaponeses/ o ajusta la ruta en las celdas de carga.

## Troubleshooting rápido
- Plotly: "nbformat>=4.2.0 but it is not installed"
  - Solución: pip install nbformat
  - Configura el renderer según tu entorno:
    python
    import plotly.io as pio
    pio.renderers.default = "notebook"  # VS Code: "vscode"; Navegador: "browser"
    
- Matplotlib: TypeError con plt.show(renderer='browser')
  - matplotlib no acepta renderer en plt.show(). Usa solo plt.show().

## Paso a paso del notebook
- [Importaciones y renderer]
  - numpy, pandas, matplotlib, seaborn.
  - sklearn: StandardScaler, train_test_split, PCA, LogisticRegression, métricas.
  - plotly.express, plotly.graph_objects; plotly.io con pio.renderers.default = "notebook".

- [Carga de datos]
  - Lee test_data.csv en datos_japoneses y muestra info general (10k filas x 786 columnas).

- [Limpieza inicial]
  - Elimina columnas auxiliares ('Unnamed: 0') y gestiona label según el análisis.

- [Distribución de clases]
  - Conteo por label y gráfico de barras con Plotly.

- [Normalización y preparación]
  - Elimina duplicados y nulos.
  - Separa labels y data.
  - Normaliza por etiqueta con Z-score (scipy.stats.zscore).
  - Calcula varianza y desviación estándar por píxel y por clase.

- [PCA: varianza explicada]
  - Ajusta PCA() y grafica varianza explicada acumulada.
  - Resumen observado:
    - 20 PCs ≈ 39.8%
    - 50 PCs ≈ 60.8%
    - 100 PCs ≈ 77.4%
    - 200 PCs ≈ 90.0%
    - 300 PCs ≈ 94.7%
    - 500 PCs ≈ 98.4%

- [Reducción a 500 componentes]
  - StandardScaler sobre pixeles y PCA(n_components=500).
  - train_test_split(..., test_size=0.3, random_state=42).

- [Matriz de correlación (componentes PCA)]
  - pd.DataFrame(X_train).corr() y sns.heatmap(...).
  - Resultado observado: 0 pares con |corr| > 0.9 (componentes desacopladas).

- [Modelo base: Regresión Logística]
  - LogisticRegression(multi_class='multinomial', solver='lbfgs', max_iter=2000).
  - Accuracy de referencia ≈ 63.5%.
  - Matriz de confusión con seaborn.

- [GridSearch (opcional)]
  - Ejemplo de GridSearchCV (comentado) para buscar mejores hiperparámetros.
  - Recomendado ejecutarlo sobre un subset por costo computacional.

## Sugerencias
- Añadir scikit-learn, scipy, plotly, nbformat a requirements.txt para instalación unificada.
- Persistir modelos con joblib.
- Agregar validación cruzada y curvas ROC por clase si se amplía el trabajo.



 pip install numpy pandas matplotlib seaborn scikit-learn plotly scipy