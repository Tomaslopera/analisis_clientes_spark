# Análisis de clientes con Spark — Entregable 2

Caso de negocio: desempeño por departamento, top categorías por ciudad, cliente estrella
por ciudad y evolución mensual de ventas, resuelto con PySpark (DataFrames + Spark SQL)
sobre Databricks.

**Integrantes:** Pedro Sierra y Tomás Lopera

## Contenido del repo

- [`analisis_clientes_spark.ipynb`](analisis_clientes_spark.ipynb) — notebook con las 4 tareas (T1-T4) y la parte de optimización, con código, resultados y análisis de negocio.
- [`informe_analisis_clientes_spark.pdf`](informe_analisis_clientes_spark.pdf) — informe corto (3 páginas) con resultados, respuestas de negocio y evidencia de optimización (tiempos, plan de `explain()`, broadcast join).
- [`datos/`](datos/) — `transacciones.csv` (20.000 filas) y `ciudades.csv` (7 filas).
- [`enunciado.md`](enunciado.md) / [`LEEME.txt`](LEEME.txt) — enunciado original del entregable.

## Cómo correr el notebook

1. Sube `datos/transacciones.csv` y `datos/ciudades.csv` a un catálogo/tabla en Databricks (o ajústalo para leer directamente los CSV con `spark.read.csv`).
2. Abre `analisis_clientes_spark.ipynb` en Databricks y corre todas las celdas ("Run all").
3. La sesión `spark` ya existe en el cluster de Databricks — no requiere instalar PySpark ni crear `SparkSession`.

> Nota: `cache()`/`persist()` no está soportado en compute **Serverless** de Databricks
> (`NOT_SUPPORTED_WITH_SERVERLESS`). Si tu workspace solo tiene Serverless, el notebook
> captura ese error y sigue corriendo; para medir la mejora de cache en la práctica, usa
> un clúster clásico (All-Purpose).

## Resumen de resultados

- **T1:** Cundinamarca y Antioquia concentran ~65% de las ventas; el ticket promedio varía poco entre regiones (~9%).
- **T2:** Electrónica lidera en las 7 ciudades sin excepción; Barranquilla es la única donde ropa supera a hogar en el 2º puesto.
- **T3:** El cliente estrella gasta entre 3.6x y 9.3x el promedio de su ciudad, según el tamaño de la plaza.
- **T4:** El acumulado supera $1,000,000 en mayo de 2025; ventas estables durante el año, sin estacionalidad marcada.
