# 🎵 Pitchfork Reviews — EDA

Proyecto de análisis exploratorio sobre el histórico de reseñas musicales de **Pitchfork** (1999–2017). A partir de 18.393 registros, se realiza una limpieza rigurosa del dataset, se calculan métricas descriptivas sobre las puntuaciones y se analizan patrones temporales y outliers. El proyecto sienta la base para análisis posteriores de sesgos y rankings.

---

## ✨ Características principales

- Carga y exploración inicial del dataset (`reviews.csv`) con detección de tipos de datos, dimensiones y cardinalidades
- Detección y eliminación de duplicados por `reviewid` (4 registros duplicados identificados)
- Tratamiento de nulos: eliminación de 3 filas con `title` o `artist` nulo; `author_type` conservado con nulos documentados (21,23%)
- Cálculo de métricas estadísticas del campo `score`: media, mediana, moda, desviación estándar, máximo y mínimo
- Identificación de los 10 álbumes mejor y peor puntuados
- Detección de outliers mediante criterio IQR estricto (factor 3×IQR): 200 outliers identificados (1,09% del total)
- Visualizaciones: distribución de scores (histograma), evolución del score medio por año (1999–2017) y boxplot

---

## 🗂️ Estructura del proyecto

```
Pitchfork/
│
├── notebooks/
│   ├── 01_eda.ipynb          # Carga, limpieza, métricas y visualizaciones
│   ├── 02_bias_tests.ipynb   # Análisis de sesgos
│   └── 03_rankings.ipynb     # Rankings de artistas y álbumes
│
├── reviews.csv               # Dataset original de reseñas Pitchfork
└── README.md
```

---

## 🛠️ Tecnologías y requisitos

| Herramienta | Versión | Uso principal |
|---|---|---|
| Python | 3.13 | Lenguaje base |
| Pandas | ≥ 2.0 | Carga, limpieza y análisis del dataset |
| NumPy | ≥ 1.24 | Operaciones numéricas |
| Matplotlib | ≥ 3.7 | Visualizaciones estáticas |
| SciPy | ≥ 1.10 | Estadística descriptiva e inferencial |
| PyArrow | ≥ 12.0 | Soporte de tipos y formatos de datos |
| Scikit-learn | ≥ 1.2 | Imputación, PCA y clasificación (KNN) |

---

## ⚙️ Instalación

```bash
python3 -m venv .venv
source .venv/bin/activate        # En Windows: .venv\Scripts\activate
pip install pandas numpy matplotlib scipy pyarrow scikit-learn jupyter
```

---

## 🚀 Uso

Ejecuta los notebooks en orden desde el directorio `notebooks/`:

```bash
jupyter notebook
```

**Carga del dataset:**

```python
import pandas as pd

df = pd.read_csv("../reviews.csv")
df.head()
```

**Eliminación de duplicados por reviewid:**

```python
df_limpio = df.drop_duplicates(subset=['reviewid'], keep='first')
```

**Detección de outliers con IQR estricto:**

```python
Q1 = df["score"].quantile(0.25)
Q3 = df["score"].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 3 * IQR
upper_bound = min(Q3 + 3 * IQR, 10)

df["is_outlier"] = (df["score"] < lower_bound) | (df["score"] >= upper_bound)
```

---

## 📊 Datos

El dataset `reviews.csv` contiene reseñas de álbumes publicadas en Pitchfork entre 1999 y 2017.

| Variable | Tipo | Descripción |
|---|---|---|
| `reviewid` | int | Identificador único de la reseña |
| `title` | str | Título del álbum |
| `artist` | str | Nombre del artista |
| `score` | float | Puntuación del álbum (0.0 – 10.0) |
| `best_new_music` | int | Flag: 1 si es "Best New Music", 0 si no |
| `author` | str | Nombre del crítico |
| `author_type` | str | Rol del crítico (21,23% nulos) |
| `pub_date` | date | Fecha de publicación |
| `pub_year` | int | Año de publicación (1999–2017) |
| `pub_month` | int | Mes de publicación |
| `pub_day` | int | Día de publicación |
| `pub_weekday` | int | Día de la semana (0=lunes, 6=domingo) |
| `url` | str | URL de la reseña en Pitchfork |

**Estado del dataset tras limpieza:**
- Registros originales: 18.393
- Duplicados eliminados (por `reviewid`): 4 → 18.389 registros
- Nulos en `title` o `artist` eliminados: 3 → **18.386 registros finales**

---

## 📈 Resultados

**Métricas de puntuación (sobre el dataset completo):**

| Métrica | Valor |
|---|---|
| Media | 7.01 |
| Mediana | 7.20 |
| Moda | 7.00 |
| Desviación estándar | 1.29 |
| Mínimo | 0.00 |
| Máximo | 10.00 |

**Álbumes con puntuación máxima (10.0):** Public Image Ltd – *Metal Box*, Bob Dylan – *Blood on the Tracks*, Brian Eno – *Another Green World*, entre otros.

**Álbum con puntuación mínima (0.0):** Various Artists – *This Is Next*.

**Outliers detectados (IQR × 3):** 200 registros (1,09% del total). Límite inferior: 2.2. 124 por debajo del límite, 76 con score = 10.

**Evolución temporal:** El score medio por año se mantiene estable entre 6.8 y 7.4, con una tendencia levemente alcista hacia 2016–2017.

---

📄 Licencia MIT
