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

## IMPORTANT

En una celda del archivo de colab que vallan a trabajar poner esto les clonara el github y `NO SE OLVIDEN DE QUITARLA PARA ENTREGAR UNA COPIA DEL TRABAJO FINAL`

```python
!git clone https://github.com/P34NUT2/Repo_to_context_datos_masivos.git
%cd Repo_to_context_datos_masivos
```

4. Guardar cambios de vuelta: `Archivo` -> `Guardar una copia en GitHub`.

## Cómo usar la IA con este repo (primera vez)

1. Clona el repo en Colab (paso anterior).
2. Sube o pega el contenido de `instrucciones.md` a la IA (Gemini/Claude/etc).
3. Usa este mini prompt para arrancar:

```python
hola IA puedes LEER procesar y checar el archivo de intrucciones y contexto porfa, y ayudame a resolver este ejercicio y porfavor no dejes ningun rastro de github del `Repo_to_context_datos_masivos` quiero subir este trabajo limpio porfa.

```
