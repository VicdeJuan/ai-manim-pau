# Todo: Fase 4 - Motor de Calificación (Rubric DAG)

Lógica de evaluación y feedback.

- [ ] **4.1: Esquema de Rúbrica Estructurada**
  - Campos: criteria_id, max_score, dependency_id, penalty_rules (JSON).
  - Ejemplo de rúbrica para un problema de matrices.
- [ ] **4.2: Orquestador DAG (LangGraph)**
  - Nodo que calcula el orden de ejecución de criterios.
  - Gestión de transferencia de contexto entre nodos de evaluación.
- [ ] **4.3: Agente Calificador de Criterio**
  - Prompt que recibe (LaTeX alumno, Criterio, Resultados previos).
  - Lógica de "arrastre de error" para evitar doble penalización.
- [ ] **4.4: Generador de Reporte**
  - Consolidación de JSON a feedback amigable para el alumno.
