# SparkSQLAndDataFrames

A structured, hands-on collection of **Apache Spark** notebooks covering the full data engineering and data science workflow — from loading raw data into DataFrames, through SQL querying and aggregation, to time-series analysis and machine learning with MLlib.

All notebooks are self-contained and designed to be run sequentially, progressing from foundational concepts to applied analytics.

---

## Repository Structure

```
SparkSQLAndDataFrames/
├── Data/                                              # Input datasets
│
├── Spark1_LoadDataIntoDataframes.ipynb                # Level 1 — Foundation
├── Spark1_BasicDataFrameOperations.ipynb
├── Spark1_DataFrameFiltering.ipynb
├── Spark1_DataAggregation.ipynb
│
├── Spark2_SQL4DataFrameQueries.ipynb                  # Level 2 — Spark SQL
├── Spark2_JoiningDataFramesInSQL.ipynb
│
├── Spark3_Timeseries Analysis.ipynb                   # Level 3 — Advanced Analysis
│
├── Spark4_Machine Learning - Linear Regression.ipynb  # Level 4 — MLlib
├── Spark4_Machine Learning - Clustering.ipynb
│
└── README.md
```

---

## Learning Path

The notebooks follow a deliberate four-level progression:

### Level 1 — DataFrame Fundamentals

| Notebook | What you learn |
|---|---|
| `Spark1_LoadDataIntoDataframes` | Creating a `SparkSession`, reading CSV/JSON/Parquet into DataFrames, inspecting schema and data types |
| `Spark1_BasicDataFrameOperations` | Selecting columns, renaming, casting types, adding derived columns, writing DataFrames back to disk |
| `Spark1_DataFrameFiltering` | `.filter()` / `.where()`, combining conditions, handling nulls, removing duplicates |
| `Spark1_DataAggregation` | `.groupBy()`, `.agg()`, built-in functions (`sum`, `mean`, `count`, `min`, `max`), window functions |

### Level 2 — Spark SQL

| Notebook | What you learn |
|---|---|
| `Spark2_SQL4DataFrameQueries` | Registering DataFrames as temporary views, running full SQL queries with `spark.sql()`, mixing the DataFrame API and SQL |
| `Spark2_JoiningDataFramesInSQL` | Inner, left, right, and outer joins both via DataFrame API and SQL syntax, handling join key conflicts |

### Level 3 — Time-Series Analysis

| Notebook | What you learn |
|---|---|
| `Spark3_Timeseries Analysis` | Parsing and casting date/timestamp columns, resampling, rolling windows, lag features, trend extraction at scale |

### Level 4 — Machine Learning with MLlib

| Notebook | What you learn |
|---|---|
| `Spark4_Machine Learning - Linear Regression` | Feature assembly with `VectorAssembler`, fitting `LinearRegression`, evaluating with RMSE and R², making predictions |
| `Spark4_Machine Learning - Clustering` | K-Means clustering with `KMeans`, selecting k with the elbow method, interpreting cluster centres |

---

## Prerequisites

### Software

| Component | Version |
|---|---|
| Python | 3.8+ |
| Apache Spark | 3.x |
| Java (JDK) | 8 or 11 |
| Jupyter | Any recent version |

### Python packages

```bash
pip install pyspark jupyter pandas matplotlib
```

### Java

Spark requires Java. Verify with:

```bash
java -version
```

If not installed, download [OpenJDK 11](https://adoptium.net/).

---

## Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/alketcecaj12/SparkSQLAndDataFrames.git
cd SparkSQLAndDataFrames

# 2. Install dependencies
pip install pyspark jupyter pandas matplotlib

# 3. Launch Jupyter
jupyter notebook

# 4. Open notebooks in order, starting with Level 1
```

Every notebook initialises its own `SparkSession` in the first cell — no external Spark cluster is needed. Spark runs in local mode:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("SparkSQLAndDataFrames") \
    .master("local[*]") \
    .getOrCreate()
```

---

## Key Concepts Covered

### DataFrame API vs Spark SQL

Both approaches are demonstrated throughout, so you can compare them side by side:

```python
# DataFrame API
df.filter(df['salary'] > 50000).groupBy('department').agg({'salary': 'mean'})

# Equivalent Spark SQL
spark.sql("""
    SELECT department, AVG(salary)
    FROM employees
    WHERE salary > 50000
    GROUP BY department
""")
```

### Window Functions

```python
from pyspark.sql.window import Window
from pyspark.sql import functions as F

window = Window.partitionBy('department').orderBy('salary')
df.withColumn('rank', F.rank().over(window))
```

### MLlib Pipeline Pattern

```python
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.regression import LinearRegression

assembler = VectorAssembler(inputCols=['feature1', 'feature2'], outputCol='features')
lr = LinearRegression(featuresCol='features', labelCol='target')
```

---

## Scientific Method Framework

| Step | Application in this repo |
|---|---|
| **Observation** | Raw data loaded from `Data/` folder; schema and distributions inspected |
| **Hypothesis** | Transformations, filters, and aggregations formulated as data questions |
| **Prediction** | SQL queries and ML models encode predictions about structure in the data |
| **Experiment** | Notebooks executed cell by cell, outputs verified at each step |
| **Conclusion** | Model metrics (RMSE, R², cluster inertia) evaluated and interpreted |

---

## Use Cases

This repository is particularly relevant for:

- **Data Engineers** learning the Spark DataFrame API for large-scale ETL pipelines
- **Data Scientists** moving from pandas to PySpark for distributed computation
- **Analytics professionals** who know SQL and want to apply it at scale with Spark SQL
- **Credit risk / financial analysts** building data pipelines on large loan or transaction datasets — all DataFrame patterns transfer directly to Spark-based data lakes

---

## Tech Stack

| Component | Tool |
|---|---|
| Distributed compute | [Apache Spark 3.x](https://spark.apache.org/) |
| DataFrame / SQL API | PySpark |
| Machine learning | Spark MLlib |
| Notebook environment | [Jupyter](https://jupyter.org/) |
| Data formats | CSV, JSON, Parquet |

---

## Author

**Alket Cecaj** — Data Scientist & Quantitative Risk Analyst  
[GitHub](https://github.com/alketcecaj12)

---

## License

Open source. Feel free to fork, adapt, and build on top of it.
