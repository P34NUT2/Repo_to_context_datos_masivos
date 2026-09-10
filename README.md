# Datos Masivos - Proyecto

Proyecto de la materia Datos Masivos. Trabajo con PySpark en Google Colab.

## Estructura del repo

- `instrucciones.md` — reglas y flujo de trabajo para la IA (leer antes de codear).
- `CONTEXTO.md` — plan/bitácora del proyecto, se actualiza cada sesión para no perder el hilo.
- `contexto_generado.md` — resumen que la IA genera de instrucciones.md, buenas_practicas/ y ejemplos/ (se crea la primera vez que trabajas con la IA).
- `buenas_practicas/` — buenas prácticas de Spark y conceptos clave (narrow/wide, lazy evaluation, etc.).
- `ejemplos/` — notebooks de clase del profe, solo de referencia (no copiar tal cual).
- `entregable/` — aquí va TODO el código final, carpetas y parquets generados.
- `notebooks/` — notebooks de trabajo/desarrollo.

## Flujo del proyecto

ETL -> EDA -> Limpieza -> Análisis/Transformaciones

## Cómo abrir en Colab

1. Sube este repo a GitHub.
2. En Colab: `Archivo` -> `Abrir cuaderno` -> pestaña `GitHub` -> pegar la URL del repo.
3. O clonar dentro de un notebook:

\```python
!git clone https://github.com/TU_USUARIO/TU_REPO.git
%cd TU_REPO
\```

4. Guardar cambios de vuelta: `Archivo` -> `Guardar una copia en GitHub`.

## Cómo usar la IA con este repo (primera vez)

1. Clona el repo en Colab (paso anterior).
2. Sube o pega el contenido de `instrucciones.md` a la IA (Gemini/Claude/etc).
3. Usa este mini prompt para arrancar:
