# Implementation Plan: ai-manim-pau MVP

Cada fase es una rodaja vertical funcional. Las tareas marcadas con **[👤 TÚ]** requieren decisión o trabajo humano y no pueden ser delegadas a un agente.

---

## Fase 0: Entorno y Tooling
**Objetivo:** Tener el entorno de desarrollo reproducible y los wrappers CLI operativos.

- [ ] **0.1 [👤 TÚ]** Crear `pyproject.toml` con dependencias: `manim`, `langgraph`, `langchain`, `pytest`, `pydantic`.
- [ ] **0.2 [👤 TÚ]** Verificar instalación de Manim Community: `manim --version` sin errores, renderizado de escena de prueba.
- [ ] **0.3 [👤 TÚ]** Configurar wrappers CLI: comprobar que `gemini` y `claude` (o equivalentes) están disponibles en PATH y autenticados.
- [ ] **0.4** Definir estructura de directorios del código fuente (`src/`, `tests/`, `content/`).

---

## Fase 1: Infraestructura Core y State Management (GitOps)
**Objetivo:** Establecer la base de persistencia de estado y la taxonomía de artefactos.

- [ ] **1.1** Definir y documentar el esquema JSON para `meta.json` en `docs/schemas/artifact_schema.json`. Campos: `status`, `version`, `created_at`, `updated_at`, `dependencies`, `output_files`, `pipeline`.
- [ ] **1.2** Definir el esquema de `cues.json` (marcadores de audio) en `docs/schemas/cues_schema.json`. Campos: `id`, `start_time`, `label`, `scene_id`.
- [ ] **1.3** Implementar `ArtifactManager` en Python (TDD). Funciones: `load_artifact(path)`, `update_status(path, status)`, `get_artifacts_by_status(status)`.
- [ ] **1.4** Setup inicial de LangGraph: configurar SQLite checkpointer, crear grafo "Hello World" que use `ArtifactManager` para escribir un estado inicial y leerlo.
- [ ] **1.5 [👤 TÚ]** Refinar `latex/pau_style.sty`: ajustar espaciados, tipografía y cabecera hasta que sea visualmente consistente con el estilo del IES Francisco de Goya. Compilar sin warnings en cada iteración.

---

## Fase 2: Motor Audiovisual (Manim + Sync)
**Objetivo:** Renderizar un vídeo sincronizado con audio a partir de LaTeX, con validación humana del storyboard.

- [ ] **2.1** Implementar `SyncManager` (TDD). Métodos: `load_cues(json_path)`, `play_until(cue_id, animation)`, `get_duration(cue_id)`.
- [ ] **2.2** Implementar Librería de Componentes MVP (`PAUScene`, `MathText`, `GraficoPAU`, `ExplicacionEcuacion`). Incluye auto-encuadre y cámaras predefinidas.
- [ ] **2.3** Implementar nodo LangGraph **Agente Director AV**: recibe LaTeX, segmenta en escenas (máx. 90s), genera directrices narrativas y selecciona metáforas visuales del catálogo `METAPHORS.md`.
- [ ] **2.4** Implementar nodo LangGraph **Agente Productor AV**: genera locución (MP3) y el `cues.json` con marcadores de tiempo. Es el primer nodo del pipeline AV porque el audio dicta el ritmo.
- [ ] **2.5** Implementar nodo LangGraph **Agente Trabajador Manim**: genera código Python usando `PAUScene` y `SyncManager`. Nunca usa tiempos hardcodeados.
- [ ] **2.6** Implementar nodo LangGraph **Agente Evaluador Visión**: renderiza en baja calidad varias opciones visuales y selecciona la más didáctica mediante VLM.
- [ ] **2.7** Implementar Storyboard Web: exportación HTML/JS de previsualización + `interrupt()` en LangGraph para esperar aprobación humana.
- [ ] **2.8 [👤 TÚ]** Revisar y aprobar el primer storyboard generado. Ajustar cámara y ritmo usando el panel de control del Storyboard Web.
- [ ] **2.9** Renderizado final: nodo de renderizado MP4 de alta calidad, activado tras la aprobación del paso 2.8. Escribe resultado en `meta.json`.

---

## Fase 3: Pipeline de Transcripción y Validación
**Objetivo:** Transformar imágenes de ejercicios manuscritos en LaTeX validado por el alumno.

- [ ] **3.1** Implementar nodo LangGraph **Agente Transcriptor** (TDD): recibe imagen, genera LaTeX y `confidence_score` (0.0–1.0). Lógica de umbrales: `>0.85` pasa directo, `0.60–0.85` pasa con aviso, `<0.60` activa rechazo temprano.
- [ ] **3.2** Implementar **Rechazo Temprano**: flujo que rechaza la imagen y solicita nueva captura si `confidence_score < 0.60`, sin abrir el editor. Incluir clasificación del motivo de rechazo (`LEGIBILIDAD_FISICA`, `AMBIGUEDAD_MATEMATICA`).
- [ ] **3.3 [👤 TÚ]** Decidir la librería WYSIWYG (MathLive vs MathQuill) y su método de integración (iframe, componente web).
- [ ] **3.4** Integrar editor WYSIWYG: recibir LaTeX del agente, inyectarlo en el editor, capturar la confirmación definitiva del alumno.

---

## Fase 4: Motor de Calificación (Rubric DAG)
**Objetivo:** Evaluación determinista con gestión de arrastre de error.

- [ ] **4.1 [👤 TÚ]** Crear la primera **Rúbrica Estructurada** real para un problema de Matrices (contenido educativo). Este artefacto es la entrada necesaria para validar el motor de calificación.
- [ ] **4.2** Definir y documentar el esquema JSON de la Rúbrica Estructurada en `docs/schemas/rubric_schema.json`. Campos: `criteria_id`, `max_score`, `dependency_ids`, `penalty_rules`, `auto_fail_conditions`.
- [ ] **4.3** Implementar orquestador DAG en LangGraph: calcula el orden de ejecución de criterios según dependencias, transfiere contexto (informes previos) entre nodos.
- [ ] **4.4** Implementar nodo LangGraph **Agente Calificador de Criterio** (TDD): recibe (LaTeX del alumno, criterio, informes previos), aplica regla de no doble penalización, devuelve puntuación y justificación.
- [ ] **4.5** Implementar nodo LangGraph **Agente Director Principal**: divide un examen completo en ejercicios y asigna cada uno a un Agente Director de Ejercicio.
- [ ] **4.6** Implementar **Generador de Informe Consolidado**: une calificaciones por criterio en feedback amigable para el alumno y nota global.

---

## Fase 5: Integración y Dashboard
**Objetivo:** Unificar los dos pipelines y visualizar el estado del sistema.

- [ ] **5.1** Crear el Grafo LangGraph principal que conecte AV Pipeline y Grading Pipeline, usando `ArtifactManager` como nexo de estado.
- [ ] **5.2** Desarrollar Dashboard Streamlit que lea exclusivamente los `meta.json` y muestre el estado de cada artefacto (ej. "Pte. Locución", "Pte. Revisión Humana", "Aprobado").
- [ ] **5.3 [👤 TÚ]** Test E2E supervisado: generación de vídeo y corrección de un examen completo de Matrices. Validar storyboard y transcripción manualmente.
- [ ] **5.4** Test E2E automatizado: script de pytest que simula el flujo completo con fixtures de imagen y LaTeX, sin intervención humana, verificando el DAG de corrección.
