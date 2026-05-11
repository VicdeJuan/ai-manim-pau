---
name: rubric-grader
description: Evaluates atomic math criteria using structured rubrics and error carry-forward logic. Use when grading student exercises, applying specific rubric penalties, or generating justified pedagogical feedback.
---

# Skill: Rubric Grader (Evaluador por Rúbrica)

Esta skill define el procedimiento para calificar un único criterio de evaluación de forma justa, objetiva y pedagógica, siguiendo los estándares de los tribunales de la PAU.

## Responsabilidades
- **Evaluación Atómica**: Calificar exclusivamente el criterio asignado, ignorando errores u aciertos fuera de su ámbito.
- **Gestión de Arrastre de Error**: Analizar si un fallo en este paso es consecuencia de un error previo ya penalizado.
- **Aplicación de Penalizaciones**: Consultar el catálogo de "Errores Comunes" de la rúbrica y aplicar la resta de puntos exacta.
- **Justificación de Calificación**: Explicar al alumno el "por qué" de su nota basándose en la rúbrica.
## Workflow de Calificación (TDD Obligatorio)

1.  **Definición de Test de Caso (RED)**: Crea un test que incluya un input (LaTeX del alumno) y el output esperado (puntuación y feedback) basándose en la rúbrica, contemplando específicamente el arrastre de error si aplica.
2.  **Carga de Contexto (DAG) (GREEN)**:
    - Leer el **Criterio Actual** de la `Rúbrica Estructurada`.
    - Leer los **Informes Previos** (si existen) de los criterios de los que este depende.
3.  **Validación de Premisas**:
...
    - Si el criterio anterior falló, verificar si el alumno ha continuado el ejercicio de forma coherente con su error anterior.
3.  **Evaluación de Ejecución**:
    - Comparar la resolución del alumno con la "Resolución Ideal" adaptada (si hay arrastre de error).
    - Identificar discrepancias matemáticas.
4.  **Asignación de Puntuación**:
    - **Caso Perfecto**: Puntuación máxima.
    - **Error de Cálculo Leve**: Aplicar penalización estándar (ej. -0.25).
    - **Error de Concepto**: Aplicar penalización mayor o 0 en el criterio.
    - **Arrastre de Error**: Si el procedimiento es correcto sobre datos erróneos heredados, otorgar puntuación máxima en este criterio.
5.  **Generación de Feedback**:
    - Redactar un comentario breve y motivador que identifique el error específico.

## Referencias
- **Modelo de Arrastre de Error**: Ver [ERROR_CARRYOVER.md](./references/ERROR_CARRYOVER.md).
- **Esquema de Rúbrica JSON**: Ver [RUBRIC_SCHEMA.md](./references/RUBRIC_SCHEMA.md).

## Triggers
- Se activa cuando el `Agente Director de Ejercicio` asigna un criterio para evaluar.
- Se activa en una fase de revisión si se detecta una inconsistencia en la nota final.
