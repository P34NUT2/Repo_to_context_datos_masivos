# Conceptos clave de Spark (de la presentación del curso)

## Transformaciones Narrow vs Wide
- **Narrow**: cada partición del resultado se genera a partir de una sola
  partición del dato original (ej. `select`, `filter`, `withColumn`). No
  requiere mover datos entre nodos.
- **Wide**: la partición del resultado depende de varias particiones del
  dato original (ej. `groupBy`, `join`, `orderBy`, `distinct`). Esto genera
  un **shuffle** (los datos se reparten entre nodos), que es costoso.

Por esto conviene minimizar operaciones wide y, cuando sean necesarias,
aplicarlas después de reducir el volumen de datos con filtros y selección
de columnas.

## Lazy Evaluation (evaluación perezosa)
- Las **transformaciones** (`select`, `filter`, `withColumn`, `join`, etc.)
  no se ejecutan al momento de escribirlas. Solo se agregan a un plan de
  ejecución.
- Las **acciones** (`show`, `count`, `collect`, `write`) son las que
  disparan la ejecución real de todo el plan acumulado hasta ese punto.

Consecuencia práctica: si llamas varias acciones sobre el mismo DataFrame
sin cachearlo, Spark recalcula todo el plan desde el inicio cada vez. Por
eso `cache()`/`persist()` importa cuando un DataFrame se reutiliza.

## RDD vs DataFrame vs Dataset
- **RDD**: la abstracción más básica, control fino pero sin optimización
  automática (no usa el optimizador Catalyst).
- **DataFrame**: datos organizados en columnas con tipos, optimizado por
  Spark automáticamente. Es lo que se debe usar en este curso.
- **Dataset**: como DataFrame pero con tipado fuerte (más común en Scala/
  Java, poco usado en PySpark).
