# Implementation Plan: ai-manim-pau MVP

Este plan detalla las fases de desarrollo para construir el motor de automatización. Cada fase está diseñada para ser una rodaja vertical funcional.

## Fase 1: Infraestructura Core y State Management (GitOps)
**Objetivo:** Establecer la base del sistema de archivos y la gestión de estados.

- [ ] **Task 1.1:** Definir el esquema JSON para `meta.json` (Artefactos).
- [ ] **Task 1.2:** Crear un módulo de utilidades `ArtifactManager` para leer/escribir estados en la jerarquía de directorios.
- [ ] **Task 1.3:** Setup inicial de LangGraph con persistencia en SQLite para pruebas de flujo.
- [ ] **Task 1.4:** Refinar la plantilla LaTeX (`pau_style.sty`) para ajustar estética y legibilidad final.

## Fase 2: Motor Audiovisual (Manim + Sync)
**Objetivo:** Renderizar un vídeo sincronizado a partir de marcadores.

- [ ] **Task 2.1:** Crear la clase base `SyncManager` para Manim.
- [ ] **Task 2.2:** Implementar la "Librería de Componentes" básica (Abstracción sobre Manim para PAU).
- [ ] **Task 2.3:** Crear el Agente Trabajador (Manim) capaz de usar `SyncManager`.
- [ ] **Task 2.4:** Implementar el prototipo de "Storyboard Web" (exportación simple HTML/JS).

## Fase 3: Pipeline de Transcripción y Validación
**Objetivo:** Transformar imágenes de alumnos en LaTeX editable y validado.

- [ ] **Task 3.1:** Implementar Agente de Transcripción con lógica de "Confidence Score".
- [ ] **Task 3.2:** Desarrollar el mecanismo de "Rechazo Temprano" (Early Rejection).
- [ ] **Task 3.3:** Integrar editor WYSIWYG matemático para validación del alumno.

## Fase 4: Motor de Calificación (Rubric DAG)
**Objetivo:** Evaluación determinista con gestión de arrastre de error.

- [ ] **Task 4.1:** Definir el esquema JSON para la `Rúbrica Estructurada` (incluyendo dependencias DAG).
- [ ] **Task 4.2:** Implementar el orquestador de evaluación (Agente Director de Ejercicio).
- [ ] **Task 4.3:** Desarrollar los Agentes Calificadores especializados con lógica de "No Doble Penalización".
- [ ] **Task 4.4:** Generador de Informe Consolidado y Feedback.

## Fase 5: Integración y Dashboard
**Objetivo:** Unificar todo el sistema y visualizar estados.

- [ ] **Task 5.1:** Crear el Grafo de LangGraph principal que conecte todas las fases.
- [ ] **Task 5.2:** Desarrollar un Dashboard minimalista (Streamlit o similar) que lea los `meta.json`.
- [ ] **Task 5.3:** Test E2E: Generación de vídeo y Corrección de un examen completo de Matrices.
