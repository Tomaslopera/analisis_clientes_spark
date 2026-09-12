**BIG DATA  ·  ENTREGABLE 2  (SEGUNDA NOTA)**

**Caso de negocio: análisis de clientes con Spark**

*Resuelve preguntas de negocio reales sobre datos de transacciones, usando Spark*

Prof. Alejandro Mery Agudelo  ·  En parejas

## **El contexto**

Eres analista de datos en una cadena de comercio. La gerencia quiere entender el comportamiento de sus clientes y el desempeño por región. Te entregan los datos de transacciones del año y te piden cuatro análisis. Los resolverás con Spark (DataFrames y/o Spark SQL), aplicando lo aprendido en las clases 7 a 10\.

| El contraste con el Entregable 1 En el primer entregable resolviste consultas parecidas escribiendo MapReduce a mano: mappers, reducers, dos jobs encadenados, Docker, YARN. Aquí harás análisis MÁS complejos (joins, ventanas, varias métricas) con una fracción del código. Ese contraste es parte del aprendizaje: verás por qué la industria trabaja con Spark. |
| :---- |

## **Los datos**

Dos archivos (incluidos). Nota que ahora las transacciones NO traen el nombre de la ciudad, solo un id: tendrás que hacer un JOIN con el catálogo.

**transacciones.csv**  (20.000 filas): id\_tx, id\_cliente, id\_ciudad, categoria, monto, fecha

**ciudades.csv**  (7 filas): id\_ciudad, ciudad, departamento

| Cargar los datos en Spark df \= spark.read.csv("transacciones.csv", header=True, inferSchema=True) ciu \= spark.read.csv("ciudades.csv", header=True, inferSchema=True) Puedes trabajar con la API de DataFrames, con Spark SQL (createOrReplaceTempView), o mezclar ambas. |
| :---- |

| T1 | Desempeño por departamento *JOIN \+ agregación  ·  clases 8 y 10* |
| :---: | :---- |

**Pregunta de negocio:** ¿cómo se desempeña cada departamento? Para cada uno, calcula las ventas totales, el ticket promedio y el número de ventas.

**Entrega:** el código, la tabla de resultados ordenada por ventas, y responde: ¿qué departamento vende más? ¿el ticket promedio varía mucho entre regiones?

| T2 | Top 3 categorías por ciudad *Función de ventana (ROW\_NUMBER)  ·  clase 10* |
| :---: | :---- |

**Pregunta de negocio:** ¿cuáles son las 3 categorías que más venden en cada ciudad? La gerencia quiere adaptar el inventario por región.

| Pista técnica Esto NO se resuelve con un simple GROUP BY, porque quieres el top-3 DENTRO de cada ciudad conservando el ranking. |
| :---- |

**Entrega:** el código, el top-3 por ciudad, y responde: ¿la categoría líder es la misma en todas las ciudades o hay diferencias regionales?

| T3 | Cliente estrella por ciudad *Ventana \+ JOIN  ·  clases 8 y 10* |
| :---: | :---- |

**Pregunta de negocio:** ¿quién es el mejor cliente (el que más ha gastado en total) en cada ciudad? Se quiere lanzar un programa de fidelización.

**Entrega:** el código, la tabla (una fila por ciudad con su mejor cliente y su gasto), y responde: ¿cuánto gastó el mejor cliente frente al promedio de su ciudad?

| T4 | Evolución mensual de ventas *Ventana con acumulado (SUM OVER)  ·  clase 10* |
| :---: | :---- |

**Pregunta de negocio:** ¿cómo evolucionaron las ventas mes a mes, y cuál es el acumulado del año? Es para el reporte anual de la gerencia.

| Pista técnica Extrae el mes de la fecha (los primeros 7 caracteres: '2025-03'). Agrupa por mes y suma. Luego usa una ventana SUM(ventas\_mes) OVER (ORDER BY mes) para el acumulado progresivo. |
| :---- |

**Entrega:** el código, la tabla de 12 meses con ventas mensuales y acumulado, y responde: ¿hay meses claramente más fuertes o débiles? ¿Qué mes se superó el millón acumulado?

# **Parte de optimización (obligatoria)**

Además de resolver las 4 tareas, demuestra que sabes optimizar (clase 9). Elige UNA de tus consultas y:

1. Identifica un cálculo que se reutilice y aplícale cache(); mide el tiempo con y sin cache.

2. Ejecuta explain() sobre una consulta y señala en el plan una optimización que hizo Catalyst (por ejemplo, un filtro combinado o empujado hacia abajo).

3. Explica en qué tarea usaste el broadcast join y por qué era el caso adecuado.

# **Qué y cómo entregar**

1. Un notebook (.ipynb) con las 4 tareas resueltas y la parte de optimización, con celdas de código ejecutables y sus resultados visibles.

2. Un informe corto en PDF (máx. 4 páginas) con: los resultados de cada tarea, las respuestas a las preguntas de negocio, la evidencia de optimización (tiempos, captura del explain) y los nombres de la pareja.

3. Ambos integrantes deben entender todo el código.

| Consejos • Puedes resolver con DataFrames, con Spark SQL, o combinando ambos: elige lo que te resulte más claro. • Verifica que tus resultados tengan sentido de negocio (por ejemplo, que las ventas por departamento sumen el total general). • La calidad de las RESPUESTAS de negocio cuenta tanto como que el código corra: no basta con mostrar tablas, hay que interpretarlas. |
| :---- |

