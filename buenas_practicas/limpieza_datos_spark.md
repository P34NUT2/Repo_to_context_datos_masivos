# Recomendaciones para LIMPIEZA DE DATOS (Para trabajar con Spark)

*Datos limpios = análisis confiables + mejor rendimiento. Una buena limpieza reduce errores, mejora la calidad de los resultados y optimiza el procesamiento en Spark.*

---

## 1. CONOCE TUS DATOS
* Explora el esquema y las muestras.
* Entiende el significado de cada columna.
* Verifica tipos de datos y rangos esperados.

## 2. MANEJO DE VALORES NULOS
* Identifica columnas con nulos (`count`, `isNull`).
* Decide: eliminar, imputar o reemplazar.
* Usa `fill()`, `na.drop()` o funciones de imputación.

## 3. TIPOS DE DATOS CORRECTOS
* Asegúrate de que cada columna tenga el tipo de dato adecuado.
* Evita trabajar con todo como string.
* Usa `cast()` para convertir tipos.

## 4. ELIMINAR DUPLICADOS
* Detecta duplicados usando subset y/o todas las columnas.
* Usa `dropDuplicates()` o `distinct()`.
* Revisa llaves naturales o IDs únicos.

## 5. ESTANDARIZA FORMATOS
* Uniformiza mayúsculas/minúsculas.
* Estandariza espacios en blanco.
* Normaliza fechas y horas (`yyyy-MM-dd`).
* Usa `trim()`, `lower()`, `upper()`, `regexp_replace()`.

## 6. VALIDA REGLAS DE NEGOCIO
* Verifica rangos válidos (ej. edades, montos, porcentajes).
* Usa filtros para valores inválidos.
* Documenta las reglas aplicadas.

## 7. TRATA OUTLIERS Y ERRORES
* Detecta outliers con estadísticas (percentiles, z-score, IQR).
* Decide: corregir, acotar o eliminar.
* Revisa valores atípicos con contexto de negocio.

## 8. PARTICIONA Y OPTIMIZA
* Reparte los datos adecuadamente (`repartition`, `coalesce`).
* Persiste solo cuando sea necesario (`cache`, `persist`).
* Considera particionar por columnas de filtro frecuentes.

## 9. REGISTRA Y PROCESA DE FORMA REPRODUCIBLE
* Mantén un log de las transformaciones aplicadas.
* Usa funciones reutilizables y pipelines.
* Versiona tu código y valida resultados.

---

### 💡 TIPS SPARK
* Evita `collect()` en grandes volúmenes.
* Prefiere operaciones nativas de Spark SQL.
* Filtra y selecciona columnas antes de procesar.
* Monitorea el plan de ejecución (`explain()`).

---

### 🔄 FLUJO SUGERIDO
1. **Explorar datos** 
2. ➡️ **Limpiar y transformar** 
3. ➡️ **Validar** 
4. ➡️ **Usar en análisis/modelos**

> *Datos limpios hoy, mejores decisiones mañana.*
