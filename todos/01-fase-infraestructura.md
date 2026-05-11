# Todo: Fase 1 - Infraestructura Core y State Management

Estado inicial del proyecto. Foco en la persistencia de datos y el flujo base.

- [ ] **1.1: Esquema de meta.json**
  - Definir campos: status, version, created_at, updated_at, dependencies, output_files.
  - Documentar el esquema en `docs/schemas/artifact.json`.
- [ ] **1.2: ArtifactManager Utility**
  - Implementar en Python.
  - Funciones: `load_artifact(path)`, `update_status(path, status)`, `get_pending_artifacts()`.
- [ ] **1.3: LangGraph Setup**
  - Inicializar entorno Python con LangGraph.
  - Configurar SQLite saver para persistencia de hilos.
  - Crear un "Hello World" de grafo que use `ArtifactManager` para escribir un status inicial.
- [ ] **1.4: Refinamiento de Plantilla LaTeX**
  - Iterar sobre `pau_style.sty` para ajustar espaciados, fuentes y diseño de cabecera.
  - Validar compilación sin warnings en cada iteración.
  - Asegurar consistencia visual con el estilo del IES Francisco de Goya.
