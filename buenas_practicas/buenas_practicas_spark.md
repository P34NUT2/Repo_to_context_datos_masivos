# Buenas prácticas de desarrollo en Spark
*Código más eficiente | Menor costo | Mejor rendimiento*

## 1. Selecciona solo las columnas que vas a usar
Evita leer y procesar columnas innecesarias.

**❌ Mala práctica**
```python
df = spark.read.parquet("ruta")
```

**✅ Buena práctica**
```python
df = spark.read.parquet("ruta") \
    .select("id", "fecha", "monto")
```

## 2. Aplica filtros lo antes posible
Filtra los datos en las primeras etapas para reducir el volumen de procesamiento.

**❌ Mala práctica**
```python
df = spark.read.parquet("ruta")
# ... muchas transformaciones ...
df = df.filter(df.fecha >= "2024-01-01")
```

**✅ Buena práctica**
```python
df = spark.read.parquet("ruta") \
    .filter(df.fecha >= "2024-01-01")
# ... siguientes transformaciones ...
```

## 3. Usa broadcast para tablas pequeñas
Si una tabla es pequeña, usa broadcast para evitar shuffles en los joins.

**❌ Mala práctica**
```python
df = fact.join(dim, "id")
```

**✅ Buena práctica**
```python
from pyspark.sql.functions import broadcast
df = fact.join(broadcast(dim), "id")
```

## 4. Evita usar múltiples withColumn seguidos (usa un select)
Define todas las transformaciones en un solo select (o withColumns).

**❌ Mala práctica**
```python
df = df.withColumn("a", expr1)
df = df.withColumn("b", expr2)
```

**✅ Buena práctica**
```python
df = df.select(
    "*",
    expr1.alias("a"),
    expr2.alias("b")
)
```

## 5. Reutiliza DataFrames con caché cuando sea necesario
Si vas a usar un DataFrame varias veces, persístelo en memoria/disk.

**❌ Mala práctica**
```python
df = calcular_df()
# se usa varias veces
resultado1 = df.groupBy("x").count()
resultado2 = df.groupBy("y").count()
```

**✅ Buena práctica**
```python
df = calcular_df().cache()
resultado1 = df.groupBy("x").count()
resultado2 = df.groupBy("y").count()
# ...
df.unpersist() # liberar memoria
```

## 6. Elige el tipo de join correcto
Usa el tipo de join que realmente necesitas (inner, left, semi, anti, etc.).

**❌ Mala práctica**
```python
df = a.join(b, "id", "outer")
```

**✅ Buena práctica**
```python
# Si solo necesitas registros de 'a' que existen en 'b'
df = a.join(b, "id", "left_semi")
```

## 7. Evita UDFs cuando sea posible
Las funciones nativas de Spark son más rápidas y permiten optimizaciones del motor.

**❌ Mala práctica**
```python
from pyspark.sql.functions import udf
@udf("int")
def sumar(x, y):
    return x + y

df = df.withColumn("total", sumar(col("a"), col("b")))
```

**✅ Buena práctica**
```python
from pyspark.sql.functions import col
df = df.withColumn("total", col("a") + col("b"))
```

## 8. Usa particionamiento adecuado al escribir
Particiona por columnas de alta cardinalidad y que se usen en filtros.

**❌ Mala práctica**
```python
df.write.mode("overwrite").parquet("ruta")
```

**✅ Buena práctica**
```python
df.write.mode("overwrite") \
    .partitionBy("fecha") \
    .parquet("ruta")
```

## 9. Evita shuffles innecesarios
Los shuffles son costosos. Reduce su uso con buenas estrategias (broadcast, partición, reutilización de claves, etc.).

**❌ Mala práctica**
```python
df = df.repartition(200) # sin necesidad
```

**✅ Buena práctica**
```python
# Solo si es necesario y con un número razonable
df = df.repartition("fecha")
```

## 10. Usa formatos de almacenamiento eficientes
Prefiere formatos columnares como Parquet o ORC en lugar de CSV o JSON.

**❌ Mala práctica**
```python
df.write.csv("ruta")
```

**✅ Buena práctica**
```python
df.write.parquet("ruta")
```

## 11. Ajusta la paralelización
Utiliza un número de particiones adecuado a tu clúster y al tamaño de los datos.

**❌ Mala práctica**
```python
df = df.repartition(1) # genera cuellos de botella
```

**✅ Buena práctica**
```python
# Un número acorde al clúster y al volumen de datos
df = df.repartition(200)
```

## 12. Monitorea y valida tu ejecución
Revisa el plan de ejecución, métricas y la UI de Spark para identificar cuellos de botella.

**❌ Mala práctica**
```python
# Ejecutar sin revisar el plan ni las métricas
df.count()
```

**✅ Buena práctica**
```python
df.explain(True)
# Revisar Spark UI (stages, shuffles, tareas)
```

## 13. Evita acciones innecesarias
Las acciones disparan la ejecución completa del plan. Úsalas solo cuando realmente necesites el resultado.

**❌ Mala práctica**
```python
df.count() # solo para verificar
print(df.collect()) # trae todos los datos al driver
```

**✅ Buena práctica**
```python
# Usa acciones solo cuando sea necesario
df.show(20) # para inspección limitada
# Evita collect() en datasets grandes
# Si solo necesitas verificar existencia:
df.limit(1).count() > 0
```

---

### Resultado (Pequeñas decisiones, grandes resultados)
- ✅ Menor tiempo de ejecución
- ✅ Menor uso de recursos
- ✅ Código más mantenible
- ✅ Procesamiento más escalable
- ✅ Ahorro en costos

> *Código inteligente hoy, datos sin límites mañana.*
