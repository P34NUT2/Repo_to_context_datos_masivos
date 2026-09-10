# Contexto del proyecto (leer primero, siempre)

## Proyecto
Datos Masivos. PySpark en Colab. Flujo: ETL -> EDA -> Limpieza -> Análisis.

## Reglas clave (resumen, detalle completo en instrucciones.md)
- Todo en Spark, nada de pandas (excepto ver DataFrame).
- Nada de spark.sql(), solo funciones (.select, .filter, .groupBy, etc).
- Nada de UDFs si hay función nativa. Nada de collect() en datasets grandes.
- Código sin comentarios, simple (KISS).
- Antes de codear: revisar datos -> dar diagnóstico -> proponer lógica ->
  preguntar cómo quiero codear -> hasta entonces escribir código.

## Estructura del repo
- instrucciones.md -> flujo completo de trabajo con la IA
- buenas_practicas/ -> reglas de Spark + conceptos (narrow/wide, lazy eval)
- ejemplos/ -> notebooks del profe, solo referencia
- entregable/ -> destino final de código y parquets

## Estado actual del proyecto
(Esta sección la vas conforme avances)
- [ ] EDA hecho
- [ ] Limpieza hecha
- [ ] Análisis ejercicio 1
- [ ] Análisis ejercicio 2

## Notas de la sesión anterior
(Aquí anotas en 1-2 líneas qué se decidió la última vez, para no repetir
la explicación completa cada vez que abres una sesión nueva)
