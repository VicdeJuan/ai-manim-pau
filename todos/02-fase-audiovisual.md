# Todo: Fase 2 - Motor Audiovisual (Manim + Sync)

Core del sistema de generación de contenido. El orden importa: el audio (Productor AV) dicta el ritmo antes de escribir código Manim.

- [ ] **2.1: SyncManager** (TDD)
  - Método `load_cues(json_path)`: carga `cues.json`.
  - Método `play_until(cue_id, animation)`: ejecuta la animación hasta el timestamp del marcador.
  - Método `get_duration(cue_id)`: devuelve la duración calculada entre dos marcadores.
  - Verificar en el test que `cue_id` desconocidos lancen error descriptivo.

- [ ] **2.2: Librería de Componentes MVP** (TDD)
  - `PAUScene(Scene)`: clase base. Inicializa `SyncManager` y aplica estilos CAM/PAU.
  - `MathText`: wrapper sobre `MathTex` con tipografía y colores del proyecto.
  - `GraficoPAU`: gráfica de función con ejes auto-encuadrados.
  - `ExplicacionEcuacion`: animación paso a paso de desarrollo algebraico.
  - Auto-encuadre: todos los componentes garantizan que sus `Mobject`s estén en cámara.
  - Referencia: `skills/manim-worker/references/COMPONENTS.md`.

- [ ] **2.3: Agente Director AV** (nodo LangGraph)
  - Input: documento LaTeX fuente (validado con `pau_style.sty`).
  - Output: lista de escenas (máx. 90s cada una) con objetivo pedagógico, metáfora visual asignada y fragmento de guion.
  - Seleccionar metáforas del catálogo: `skills/av-director/references/METAPHORS.md`.
  - Escribir el storyboard conceptual en `meta.json` con `status: STORYBOARD_GENERADO`.

- [ ] **2.4: Agente Productor AV** (nodo LangGraph)
  - Input: storyboard del Director AV.
  - Output: archivo MP3 de locución + `cues.json` con marcadores `{id, start_time, label, scene_id}`.
  - Este nodo ejecuta **antes** que el Trabajador Manim; el audio dicta los tiempos.
  - Escribir `cues.json` en el directorio del artefacto.

- [ ] **2.5: Agente Trabajador Manim** (nodo LangGraph)
  - Input: directrices del Director AV + `cues.json` del Productor AV.
  - Output: script Python que extiende `PAUScene` y usa exclusivamente `self.sync.play_until(...)`.
  - Prohibido: `self.wait(n)` con números literales, uso de `Tex`/`MathTex` directos si existe equivalente en la librería.
  - Referencia: `skills/manim-worker/SKILL.md`.

- [ ] **2.6: Agente Evaluador Visión** (nodo LangGraph)
  - Input: N renderizados de baja calidad de la misma escena con variantes de composición.
  - Output: índice de la variante más didáctica + justificación.
  - Usa VLM (Gemini con visión). Selecciona por claridad pedagógica, no por validación geométrica.

- [ ] **2.7: Storyboard Web + interrupt()** 
  - Exportar previsualización HTML/JS del vídeo seleccionado por el Evaluador Visión.
  - Implementar `interrupt()` en LangGraph: el grafo se pausa esperando la aprobación humana.
  - Panel de control con sliders de cámara y ritmo.

- [ ] **2.8 [👤 TÚ]: Revisión y aprobación del storyboard**
  - Abrir el Storyboard Web, revisar la previsualización.
  - Ajustar parámetros si es necesario y dar la aprobación final.
  - Sin esta aprobación, el renderizado final no se activa.

- [ ] **2.9: Renderizado final**
  - Nodo activado tras la aprobación del 2.8.
  - Renderiza el MP4 de alta calidad.
  - Escribe `output_files` y `status: APROBADO` en `meta.json`.
