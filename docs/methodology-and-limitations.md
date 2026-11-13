# Banking77 — Metodología, resultados y evaluación de explicaciones

## El proyecto

Desarrollé este trabajo individual para la asignatura **Sistemas Cognitivos Artificiales**, dentro de la **Maestría en Inteligencia Artificial**.

Mi objetivo fue evaluar un clasificador de intenciones bancarias y analizar la utilidad de las explicaciones generadas por un segundo modelo de lenguaje cuando el primero cometía errores.

## Diseño experimental

1. **Datos:** utilicé `PolyAI/banking77`, con **77 categorías**, **10,003 consultas de entrenamiento** y **3,080 consultas de prueba**.
2. **Análisis exploratorio:** revisé la longitud de las consultas, el vocabulario y la distribución de intenciones.
3. **Validación:** dividí el conjunto inicial en **9,002 registros para entrenamiento** y **1,001 para validación**.
4. **Clasificador:** ajusté `distilroberta-base` durante **tres épocas**, seleccionando el mejor modelo mediante **F1 macro de validación**.
5. **Evaluación final:** utilicé las **3,080 consultas de prueba** para medir la clasificación.
6. **Errores:** identifiqué **407 predicciones incorrectas** y seleccioné **20 ejemplos** para analizar las explicaciones.
7. **Modelo generativo:** utilicé `tiiuae/falcon-7b-instruct` con **cuantización de 4 bits**.
8. **Revisión:** evalué las 20 explicaciones con **cuatro criterios binarios**.

## Clasificación de intenciones

| Métrica | Resultado |
| --- | ---: |
| Accuracy | **86.79 %** |
| Precisión macro | **88.38 %** |
| Recall macro | **86.79 %** |
| F1 macro | **86.02 %** |
| Aciertos | 2,673 |
| Errores | 407 |

## Evaluación manual de Falcon-7B-Instruct

| Criterio | Casos satisfactorios | Porcentaje |
| --- | ---: | ---: |
| Coherencia | 20/20 | 100 % |
| Fundamentación en la consulta | 12/20 | 60 % |
| Explicación de la confusión | 6/20 | 30 % |
| Cumplimiento del formato | 18/20 | 90 % |

**Interpretación:** un LLM puede producir textos coherentes sin explicar de forma fiable el error de otro modelo. Falcon recibió la consulta y ambas etiquetas, pero no los parámetros ni los estados internos del clasificador. Por lo tanto, las respuestas son **explicaciones post hoc**, no atribuciones causales verificadas.

La revisión de **20 casos** fue exploratoria: no permite extrapolar porcentajes al total de **407 errores** ni sustituye una evaluación con varios anotadores.

## Entorno de cómputo

El notebook se preparó para **Google Colab** con GPU **NVIDIA Tesla T4**. Falcon-7B-Instruct se cargó en 4 bits y la medición guardada registró alrededor de **13.90 GB de VRAM**.

El trabajo utilizó bibliotecas como `transformers`, `datasets`, `accelerate` y `bitsandbytes`. En la instalación original solamente `datasets==3.6.0` estaba fijado explícitamente.

## Reproducción del experimento

El [notebook publicado](../banking77_falcon_experiment.ipynb) conserva las celdas de código y las explicaciones del trabajo académico, pero sus salidas almacenadas fueron retiradas para reducir el tamaño del archivo.

Por eso las métricas documentadas corresponden a la **ejecución académica original**. Una nueva ejecución puede requerir actualizar dependencias y comprobar compatibilidad con CUDA, GPU y versiones de los modelos.

## Posibles mejoras

- Registrar un entorno de dependencias completamente reproducible.
- Incorporar verificaciones de versiones y archivos de entrada.
- Ampliar la revisión manual con más casos y evaluadores.
- Comparar explicaciones con métodos de interpretabilidad que utilicen información del propio clasificador.
- Evaluar modelos base adicionales y nuevas condiciones de prueba.

## Referencia académica

Casanueva, I., Temčinas, T., Gerz, D., Henderson, M., & Vulić, I. (2020). *Efficient intent detection with dual sentence encoders*. Proceedings of the 2nd Workshop on Natural Language Processing for Conversational AI. https://doi.org/10.18653/v1/2020.nlp4convai-1.5
