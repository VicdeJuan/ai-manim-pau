# Todo: Fase 4 - Motor de Calificación (Rubric DAG)

Lógica de evaluación determinista y feedback personalizado. La Tarea 4.1 es prerequisito humano para validar el motor.

- [ ] **4.1 [👤 TÚ]: Primera Rúbrica Estructurada real**
  - Crear la rúbrica para un problema de Matrices (ej. cálculo de determinante + inversa).
  - Definir: criterios, puntuación máxima por criterio, dependencias entre criterios (DAG), penalizaciones de errores comunes, condiciones de fallo automático.
  - Este artefacto es la entrada necesaria para probar el motor de calificación con datos reales.

- [ ] **4.2: Esquema de Rúbrica Estructurada** (JSON Schema)
  - Documentar en `docs/schemas/rubric_schema.json`.
  - Campos: `criteria_id`, `description`, `max_score`, `dependency_ids` (lista, vacía si es raíz), `penalty_rules` (lista de `{condition, points_deducted}`), `auto_fail_conditions`.
  - Referencia: `skills/rubric-grader/references/ERROR_CARRYOVER.md`.

- [ ] **4.3: Orquestador DAG** (nodo LangGraph)
  - Leer la rúbrica JSON y construir el DAG de criterios.
  - Calcular el orden topológico de evaluación.
  - Pasar a cada nodo Calificador: el criterio actual + los informes de los criterios dependientes.

- [ ] **4.4: Agente Calificador de Criterio** (TDD)
  - Input: LaTeX del alumno + criterio actual + informes previos (si los hay).
  - Output: puntuación (float) + justificación + flag `ARRASTRE_DE_ERROR` (bool).
  - Regla de no doble penalización: si el procedimiento es correcto sobre datos erróneos heredados, otorgar puntuación máxima en este criterio.
  - El test debe incluir explícitamente un caso con arrastre de error.
  - Referencia: `skills/rubric-grader/SKILL.md`.

- [ ] **4.5: Agente Director Principal** (nodo LangGraph)
  - Input: examen completo con múltiples ejercicios.
  - Output: asignación de cada ejercicio a un sub-grafo de Agente Director de Ejercicio.
  - Consolida los resultados de todos los ejercicios en el informe final.

- [ ] **4.6: Generador de Informe Consolidado**
  - Input: resultados JSON de todos los criterios de todos los ejercicios.
  - Output: informe en formato amigable para el alumno (nota global, desglose por criterio, feedback motivador).
  - Escribir resultado en `meta.json` con `status: CALIFICADO`.
