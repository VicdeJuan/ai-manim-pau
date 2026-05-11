---
name: math-transcriber
description: Transcribes handwritten math images to LaTeX and manages transcription confidence. Use when a student submits a handwritten exercise or when validating OCR output quality and applying early rejection rules.
---

# Skill: Math Transcriber (Especialista en Transcripción Matemática)

Esta skill define el estándar para interpretar las entregas manuscritas de los alumnos y convertirlas en un formato digital procesable (LaTeX), priorizando siempre la experiencia del usuario (UX) mediante la detección de fallos de lectura.

## Responsabilidades
- **Transcripción de Imagen a LaTeX**: Traducir escritura manual (posiblemente desordenada) en código LaTeX válido.
- **Evaluación de Confianza**: Asignar un valor numérico (0.0 a 1.0) a la calidad de la transcripción.
- **Protocolo de Rechazo Temprano**: Decidir si una imagen es apta para ser editada por el alumno o debe ser rechazada inmediatamente.
- **Normalización**: Limpiar el LaTeX generado para que sea compatible con el editor WYSIWYG y los agentes calificadores.
## Workflow de Transcripción (TDD Obligatorio)

1.  **Definición de Test de Transcripción (RED)**: Antes de procesar la imagen, define el resultado esperado (LaTeX) y el umbral de confianza mínimo para una imagen de prueba similar.
2.  **Análisis de Legibilidad (GREEN)**:
    - Verificar resolución, iluminación y encuadre.
    - Si la imagen es ilegible por factores externos, rechazar con motivo: `LEGIBILIDAD_FISICA`.
3.  **Generación de LaTeX y Confianza**:
...
    - Generar el LaTeX de transcripción.
    - Calcular el `confidence_score`. Este cálculo debe considerar:
        - Ambigüedad de caracteres (ej. ¿es un `2` o una `z`?).
        - Coherencia matemática (ej. ¿las ecuaciones tienen sentido estructural?).
3.  **Decisión de Filtro**:
    - **Confianza > 0.85**: Pasar directamente al editor WYSIWYG.
    - **0.60 < Confianza < 0.85**: Pasar al editor con un aviso al alumno: "Hemos tenido dificultades en algunas zonas, por favor revisa con cuidado".
    - **Confianza < 0.60**: Activar **Rechazo Temprano**. No abrir el editor. Solicitar nueva foto.
4.  **Inyección en Editor**: Formatear el LaTeX para que sea "humano-editable" (evitar macros complejas o anidamientos innecesarios).

## Referencias
- **Guía de Fallos Comunes de OCR**: Ver [OCR_PITFALLS.md](./references/OCR_PITFALLS.md).
- **Estándar de LaTeX para Edición**: Ver [LATEX_STYLE.md](./references/LATEX_STYLE.md).

## Triggers
- Se activa al recibir una nueva `Entrega` (imagen) de un alumno.
- Se activa si el alumno indica que la transcripción mostrada en el editor no se parece en nada a su original.
