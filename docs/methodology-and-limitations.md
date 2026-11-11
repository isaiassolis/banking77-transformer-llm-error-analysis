# Metodología, entorno y limitaciones

## Origen

Proyecto académico individual de la asignatura **Sistemas Cognitivos Artificiales** de la Maestría en Inteligencia Artificial. La fuente de este repositorio es el notebook entregado por su autor; no se han ejecutado nuevos entrenamientos al preparar el portafolio.

## Diseño experimental reportado

1. Carga de `PolyAI/banking77`: 77 categorías, 10,003 registros train y 3,080 test.
2. Análisis exploratorio de longitud, vocabulario y distribución de intenciones.
3. Separación de entrenamiento y validación: 9,002 y 1,001 registros, respectivamente.
4. Fine-tuning de `distilroberta-base` para clasificación de secuencias, tres épocas; selección por F1 macro de validación.
5. Evaluación final sobre las 3,080 consultas reservadas.
6. Identificación de 407 errores y selección de 20 ejemplos para generación de explicaciones lingüísticas.
7. Carga de `tiiuae/falcon-7b-instruct` en cuantización de 4 bits y calibración de prompt.
8. Evaluación manual de 20 explicaciones mediante cuatro criterios binarios.

## Resultados conservados en el notebook

- Accuracy: **0.8679**; precisión macro: **0.8838**; recall macro: **0.8679**; F1 macro: **0.8602**.
- Correctas: **2,673**; incorrectas: **407**.
- Falcon: coherencia **20/20**, fundamentación **12/20**, explicación de la confusión **6/20**, formato **18/20**.

## Recursos de cómputo

El notebook se preparó para Google Colab con GPU NVIDIA Tesla T4. Falcon se cargó cuantizado a 4 bits. El resultado guardado informa uso de VRAM cercano a **13.90 GB** en el momento de la medición.

## Riesgos metodológicos

- Las explicaciones de Falcon son **post hoc** y no describen las causas internas de las predicciones de DistilRoBERTa.
- La evaluación humana de 20 ejemplos es pequeña, subjetiva y no cuenta con anotadores múltiples ni acuerdo interanotador documentado.
- La muestra de errores no constituye una validación estadística representativa de todos los fallos.
- Las métricas reflejan el conjunto de prueba registrado, no desempeño en otros bancos, idiomas o condiciones de producción.
- La portabilidad del notebook puede depender de versiones específicas de `transformers`, `datasets`, `accelerate`, `bitsandbytes`, CUDA y GPU; solo `datasets==3.6.0` estaba fijado expresamente en el comando de instalación original.
- La preparación de esta carpeta eliminó las **salidas almacenadas** del notebook para reducir tamaño y evitar incluir trazas y salidas innecesarias; el código académico y las celdas narrativas permanecen, por lo que **los números del README provienen del archivo original**.

## Siguientes mejoras propuestas (no implementadas)

- Congelar entorno con archivo de dependencias probado y número de versión de CUDA.
- Incorporar prueba de reproducción y verificación de checksums.
- Ampliar evaluación manual con rúbrica y más de una persona evaluadora.
- Comparar otros modelos y un baseline sencillo.
- Publicar una demostración de inferencia solo después de validar recursos, licencias y seguridad.

## Referencia principal

Casanueva, I., Temčinas, T., Gerz, D., Henderson, M., & Vulić, I. (2020). *Efficient intent detection with dual sentence encoders*. Proceedings of the 2nd Workshop on Natural Language Processing for Conversational AI. https://doi.org/10.18653/v1/2020.nlp4convai-1.5