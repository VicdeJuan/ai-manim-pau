# Todo: Fase 2 - Motor Audiovisual (Manim + Sync)

Core del sistema de generación de contenido.

- [ ] **2.1: SyncManager Class**
  - Implementar en Python (sobre Manim Community).
  - Método `load_cues(json_path)`.
  - Método `play_until(cue_id, animation)`.
- [ ] **2.2: Librería de Componentes (MVP)**
  - Clase `PAUScene(Scene)`.
  - Componentes: `MathText`, `Graph`, `StepByStepExplanation`.
  - Estilos visuales predefinidos.
- [ ] **2.3: Agente Trabajador (Manim)**
  - Prompt especializado para generar código usando `SyncManager`.
  - Integración en el grafo de LangGraph.
- [ ] **2.4: Storyboard Web Prototype**
  - Exportación de frames o vídeo de baja resolución a un visor simple.
  - Implementar `interrupt()` en LangGraph para esperar aprobación humana.
