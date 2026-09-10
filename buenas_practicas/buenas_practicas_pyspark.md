# Guía de Buenas Prácticas y Optimización en PySpark

Esta guía documenta los principios fundamentales para el desarrollo eficiente, limpio y optimizado con PySpark, abarcando desde la gestión de memoria hasta la exploración de datos y la estructuración del código.

---

## 1. Gestión de Memoria y Optimización del Árbol de Ejecución (DAG)

### Evitar Acciones Innecesarias en Código de Producción
En PySpark, las **transformaciones** (como `.map()`, `.filter()`, `.groupBy()`) son *perezosas* (*lazy evaluation*), mientras que las **acciones** (como `.show()`, `.describe()`, `.collect()`, `.count()`) fuerzan la ejecución del Grafo Acíclico Dirigido (DAG).
* **Impacto:** Ejecutar `.show()` o `.describe()` en medio de un flujo de producción obliga a Spark a materializar los datos y recalcular el árbol de ejecución desde cero, saturando la memoria RAM del driver y de los *executors*, e incrementando drásticamente los tiempos de procesamiento.
* **Recomendación:** Utilizar estas acciones únicamente durante la fase de depuración (*debug*) y eliminarlas o comentarlas en la versión final del script.

### Omitir el Uso de `select("*")`
* **Inconveniente:** Incluir `.select("*")` es una práctica redundante que añade un paso innecesario al plan de optimización de Catalyst (el optimizador de Spark).
* **Recomendación:** Si se desean conservar todas las columnas, es preferible utilizar directamente el DataFrame original. Reserva `.select()` únicamente cuando vayas a filtrar, renombrar o transformar un subconjunto específico de columnas.

### Ajustar y Optimizar los Tipos de Datos (Data Types)
* **Inferencia de Esquema:** Al leer archivos sin esquema explícito (`inferSchema=True`), Spark tiende a asignar los tipos de datos más seguros y amplios (como `LongType` para enteros o `DoubleType` para decimales).
* **Optimización:** Asignar manualmente un esquema estricto (`StructType`) utilizando el menor tipo de dato necesario (por ejemplo, `IntegerType`, `ShortType` o `StringType` adecuadamente acotados) reduce significativamente la huella de memoria en los *executors* y acelera las operaciones de I/O y ordenamiento.

### Calcular la Volumetría para Optimizar Joins
* **Conocimiento de la Carga:** El uso de `.count()` permite determinar la volumetría de un conjunto de datos antes de realizar transformaciones complejas.
* **Estrategia de Join:** Identificar si una tabla es lo suficientemente pequeña (ej. < 10 MB - 100 MB) permite aplicar un **Broadcast Join** (`F.broadcast(df_pequeno)`). Esto envía la tabla pequeña a todos los nodos trabajadores, evitando el costoso proceso de redistribución de datos (*shuffle*) a través de la red.

---

## 2. Exploración y Limpieza de Datos (EDA)

### Procesar Exclusivamente con el Motor de Spark
* **Escalabilidad:** Toda la transformación, filtrado, agregación y limpieza de datos debe ejecutarse utilizando las API nativas de PySpark (`pyspark.sql`).
* **Uso Restringido de Pandas:** Convertir un DataFrame de Spark a Pandas (`.toPandas()`) trae todos los datos al nodo Driver. Esto solo debe hacerse para la visualización gráfica final de muestras pequeñas o tabulaciones cruzadas resumidas, nunca para procesar volúmenes masivos.

### Imprimir el Esquema Tempranamente (`printSchema()`)
* **Identificación de Errores de Origen:** Ejecutar `printSchema()` al inicio del análisis permite detectar incongruencias inmediatamente, como columnas numéricas o de fechas inferidas como `StringType` debido a la presencia de espacios en blanco, caracteres especiales o formatos inconsistentes.

### Filtrado Inverso Limpio (`.isin()` + `~`)
* **Tratamiento de Datos Sucios:** Para limpiar registros atípicos, nulos o no válidos de forma elegante, es recomendable construir una lista o subconjunto de valores válidos y aplicar la negación lógica con el operador tilde (`~`):
  ```python
  valores_correctos = ["Activo", "Pendiente", "Completado"]
  df_limpio = df.filter(~F.col("estatus").isin(valores_correctos))
  ```

### Controlar la Visualización de Registros
* **Visualización Completa:** Para evitar que Spark recorte el texto de columnas largas con puntos suspensivos (`...`), utiliza `.show(truncate=False)`.
* **Muestreo Eficiente:** Para inspecciones rápidas, limita explícitamente el número de filas (ej. `.show(5)` o `.take(5)`) para no saturar la consola ni gastar recursos computacionales innecesarios.

### Validar Compatibilidad en Casteos (`.cast()`)
* **Pérdida de Datos:** Al convertir el tipo de una columna mediante `.cast()`, cualquier valor que no sea compatible con el nuevo tipo se transformará silenciosamente en `null`.
* **Buenas Prácticas:** Se deben auditar los valores no nulos antes y después del casteo para garantizar que no haya pérdida no deseada de información.

---

## 3. Estructura, Estilo y Legibilidad del Código

### Estructurar Consultas Verticalmente
* **Legibilidad:** Las cadenas de transformaciones largas que se extienden horizontalmente dificultan la lectura y el mantenimiento.
* **Sintaxis Recomendada:** Utiliza la diagonal invertida (`\`) o parentización para organizar las transformaciones verticalmente:
  ```python
  # Opción con parentización (Recomendada en PEP8)
  df_resultado = (
      df_original
      .filter(F.col("edad") >= 18)
      .groupBy("categoria")
      .agg(F.avg("monto").alias("monto_promedio"))
  )
  ```

### Simplificar el Llamado de Funciones (`import pyspark.sql.functions as F`)
* **Escribir Código Limpio:** Importar las funciones de PySpark mediante el alias convención `F` evita la contaminación del espacio de nombres y mejora la claridad del código:
  ```python
  import pyspark.sql.functions as F
  from pyspark.sql.types import IntegerType

  df = df.withColumn("monto_entero", F.col("monto").cast(IntegerType()))
  ```

### Nombrar Columnas Calculadas (`.alias()`)
* **Claridad:** Siempre que se aplique una transformación, agregación o cálculo dentro de una consulta, asigna explícitamente un nombre representativo utilizando `.alias("nombre_columna")`.
* **Evitar Nombres Genéricos:** De lo contrario, Spark generará nombres complejos y automáticos como `avg(CAST(monto AS DOUBLE))`, lo cual dificulta la referencia a esas columnas en pasos subsecuentes.

---

## Resumen de Check List para Desarrolladores

| Área | Buena Práctica | Acción Clave |
| :--- | :--- | :--- |
| **Memoria** | Desactivar Debug | Quitar `.show()` y `.describe()` antes de desplegar |
| **Memoria** | Tipos Eficientes | Definir esquemas manuales en lugar de inferir todo |
| **Memoria** | Minimizar Shuffle | Utilizar `broadcast` joins para tablas pequeñas |
| **EDA** | Nativo Spark | No usar `.toPandas()` en grandes volúmenes |
| **EDA** | Filtrado Negativo | Usar `~F.col().isin()` para remover datos corruptos |
| **Estilo** | Alias de Funciones | Importar como `import pyspark.sql.functions as F` |
| **Estilo** | Formato Vertical | Formatear pipelines con paréntesis multilínea |
| **Estilo** | Renombrado | Aplicar `.alias()` en todas las columnas calculadas |
