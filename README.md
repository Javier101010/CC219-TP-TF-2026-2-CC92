# Avisos inmobiliarios del Perú: análisis exploratorio y línea base con NLP

Proyecto del curso **CC219 – Aplicaciones de Data Science**, sección CC92 (ciclo 2026-2). El repositorio contiene un notebook para explorar y preparar avisos inmobiliarios de Perú publicados en Properati, y probar líneas base de procesamiento de lenguaje natural (NLP).

## Objetivo

Analizar la calidad y el contenido de los avisos, preparar texto para modelado y comprobar de forma preliminar cuatro casos de uso:

1. Clasificar el tipo de inmueble a partir del título y la descripción.
2. Predecir el segmento de precio de departamentos en Lima Metropolitana.
3. Recuperar avisos relevantes mediante búsqueda semántica.
4. Extraer dormitorios, baños y áreas mencionados en el texto.

El notebook presenta resultados iniciales con TF-IDF y modelos lineales, una prueba de búsqueda con embeddings y FAISS, y reglas basadas en expresiones regulares. Estas pruebas sirven como línea base para el trabajo final.

## Estructura del repositorio

```text
.
├── code/
│   └── TP_EDA_Avisos_Inmobiliarios_PE_1.ipynb
├── data/
│   └── pe_properties.csv.xz
├── requirements.txt
└── README.md
```

`data/pe_properties.csv.xz` contiene el dataset original comprimido. Al ejecutar el notebook se genera `data/avisos_pe_limpio.csv.xz` y se guardan gráficos en `figs/`.

## Requisitos

- Python 3.12 (el notebook también puede ejecutarse en Google Colab).
- Dependencias listadas en [`requirements.txt`](requirements.txt).
- Conexión a internet la primera vez para descargar el dataset original desde Kaggle si no se encuentra en `data/`. La búsqueda semántica descarga el modelo de Hugging Face al ejecutarse.
- Para acelerar la prueba de embeddings se recomienda una GPU, aunque también puede ejecutarse en CPU.

## Instalación y ejecución

Clona el repositorio y entra a su carpeta:

```bash
git clone <URL_DEL_REPOSITORIO>
cd <CARPETA_DEL_REPOSITORIO>
```

Crea un entorno virtual e instala las dependencias:

```bash
python3.12 -m venv .venv
source .venv/bin/activate     # Windows: .venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Inicia JupyterLab y abre el notebook:

```bash
jupyter lab code/TP_EDA_Avisos_Inmobiliarios_PE_1.ipynb
```

Ejecuta las celdas en orden. El notebook busca primero `data/pe_properties.csv` o `data/pe_properties.csv.xz`; si no encuentra ninguno, intenta descargar `pe_properties.csv` del dataset público [`rmjacobsen/property-listings-for-5-south-american-countries`](https://www.kaggle.com/datasets/rmjacobsen/property-listings-for-5-south-american-countries). También incluye una descarga alternativa desde la API pública de Kaggle.

En Google Colab se puede abrir el notebook y ejecutar las celdas secuencialmente. La primera celda instala las dependencias adicionales requeridas para Colab.

## Datos y privacidad

El análisis parte de avisos publicados entre marzo de 2019 y marzo de 2020. El preprocesamiento elimina duplicados y avisos sin descripción, normaliza texto y deriva atributos a partir de la descripción. Se remueven teléfonos durante la limpieza del texto y los montos mencionados se enmascaran en la versión usada para ciertos experimentos, para reducir fuga de información.

La versión procesada se exporta comprimida en formato CSV/XZ. No se deben interpretar los resultados como una valoración inmobiliaria ni como una recomendación de compra o alquiler.

## Resultados preliminares

- Tras la limpieza, el notebook reporta **103 423 avisos únicos**.
- Para clasificar cinco tipos de inmueble, una línea base SVM con TF-IDF alcanza aproximadamente **0.94 de F1 macro**.
- Para predecir segmentos de precio, el modelo híbrido de texto y variables estructuradas alcanza aproximadamente **0.87 de F1 macro**.
- La extracción por reglas coincide mejor con los campos existentes para dormitorios y áreas que para baños; esta evaluación es preliminar.
- La búsqueda semántica encuentra avisos afines por significado, pero requiere combinarse con filtros de metadatos para respetar condiciones como operación y presupuesto.

Los resultados dependen de los datos y de la partición usada en el notebook. Consulta allí las métricas, gráficos, criterios de limpieza y limitaciones completas.

## Tecnologías

Python, pandas, NumPy, scikit-learn, NLTK, Matplotlib, Seaborn, sentence-transformers y FAISS.

