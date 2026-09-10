# Instrucciones para IA

## Sobre el proyecto
Proyecto de la materia Datos Masivos. Se trabaja con PySpark en Google Colab,
siguiendo el flujo: ETL -> EDA -> Limpieza -> Análisis/Transformaciones.

## muy importante
- No usar pandas para nada, excepto para ver el DataFrame (ej. `toPandas()`
  solo para visualizar con `data_table.DataTable`). Todo tiene que estar en
  SPARK debido a que el examen y la clase son de Spark.
- No usar ni generar código SQL dentro de Spark (`spark.sql(...)`), aunque
  Spark tenga esa función, es mala práctica aquí. En su lugar, usar las
  funciones nativas: `.select`, `.where`, `.filter`, `.groupBy`, `.join`, etc.
- No usar UDFs si existe una función nativa de Spark que resuelva lo mismo.
- No usar collect() en datasets grandes, usar show() con límite.
- Seleccionar solo las columnas necesarias antes de transformar.
- Aplicar filtros lo antes posible en el pipeline.
- Evitar múltiples withColumn seguidos, usar un solo select.
- Usar cache()/persist() solo si el DataFrame se reutiliza varias veces,
  y liberar con unpersist() después.
- Usar broadcast() en joins con tablas pequeñas.
- Guardar en formato parquet, particionando solo si la columna se usa
  seguido en filtros.

### NOTA
Sé breve, genera código sin comentarios, mantenlo simple (KISS), guiándote
de los notebooks de ejemplo.

## Cómo debes trabajar conmigo (flujo obligatorio antes de escribir código)
Antes de escribir cualquier código para resolver un ejercicio, debes:

1. Revisar los datos que le muestre o describa (schema, nulos, duplicados,
   columnas separadas que deberían unirse, tipos mal definidos, etc.)
2. Darme un resumen corto de lo que encontraste mal o desordenado en los
   datos. Ejemplo: "tienes la columna X y Y separadas pero para responder
   esto se necesitan juntas, y la columna Z tiene nulos que hay que decidir
   cómo tratar."
3. Proponerme tu lógica de solución explicada en pasos, ANTES de escribir
   código. Ejemplo: "primero filtraría por A, luego uniría con B, después
   agruparía por C para sacar el promedio." Si el usuario no tiene una idea
   clara de cómo resolverlo, ayudarlo a construir esa lógica.
4. Preguntarme cómo quiero resolverlo (si estoy de acuerdo con esa lógica,
   si prefiero otro enfoque, o si falta algo que no consideraste), y
   también cómo quiero que se codee: nombres de variables en inglés o
   español, código más extenso/explicado o más compacto.
5. Solo después de que yo confirme el enfoque, escribes el código.

## PASOS A SEGUIR:

1) Analizar y checar las buenas prácticas que están en la carpeta
   `buenas_practicas/` y seguir las buenas_practicas.

2) Analizar los códigos de clase (carpeta `ejemplos/`) solo para sacar una
   IDEA de los pasos a seguir: cómo se pasa de csv a parquet, cuántos
   parquets conviene generar, y cómo trabajar bien con los workers/
   particiones según esos ejemplos. No copiar el código tal cual, es
   referencia de estilo y estructura.

3) Hacer un buen EDA y análisis exploratorio (ver `limpieza_de_Datos.md`):
   checar que el dataset tenga lógica y sea **congruente** — que los
   títulos sean títulos y no números, que los tipos de dato sean los
   correctos, ver si hay valores null, duplicados, o si el dataset/parquet/
   csv está roto o corrupto de alguna forma, y en ese caso intentar
   arreglarlo.

4) Seguir el flujo de "Cómo debes trabajar conmigo" de arriba: dar el
   diagnóstico del EDA, proponer lógica, preguntar cómo codear, y solo
   entonces escribir el código de limpieza/análisis correspondiente.

## TODO EL CÓDIGO, CARPETAS Y PARQUETS DEJARLO EN LA CARPETA `ENTREGABLE/`

## Último PASO:

Generar un .md muy breve de cómo funciona todo el código.

## Generación de contexto (primera vez o cuando cambien los archivos base)
Antes de empezar a trabajar, lee `instrucciones.md`, todo `buenas_practicas/`
y los notebooks de `ejemplos/`. Después, escribe o actualiza el archivo
`contexto_generado.md` en la raíz con tu propio resumen de:
- Reglas clave que debes seguir (en tus palabras, breve).
- Patrones que identificaste en los notebooks de ejemplo (cómo se hace el
  ETL, cuántas particiones usan, estilo de código que siguen).
- Cualquier duda o ambigüedad que encontraste en las instrucciones.

En sesiones futuras, si existe `contexto_generado.md`, léelo primero en vez
de re-analizar todos los archivos desde cero, y solo actualízalo si algo
cambió o aprendiste algo nuevo relevante durante la sesión.

# CONTEXTO.md 
Por ultimo hay una pequeno archivo .md de CONTEXTO que puedes seguir para que no te pierdas y vallas llevando un plan IA, y recuerda todo lo final va en entregables.
