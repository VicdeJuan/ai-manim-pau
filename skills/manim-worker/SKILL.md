---
name: manim-worker
description: Generates mathematical animations using Manim Community and a custom component library. Use when translating storyboards or LaTeX into executable Manim scripts or refactoring existing animations.
---

# Skill: Manim Worker (Especialista en Animación Matemática)

Esta skill define el estándar de implementación técnica para generar vídeos educativos. El objetivo es producir código Manim Community que sea visualmente coherente, pedagógicamente claro y perfectamente sincronizado con el audio.

## Principios de Diseño
- **Modularidad**: El código debe estar organizado en secciones que correspondan a los bloques de contenido definidos por el `Agente Director`.
- **Sincronización Semántica**: Nunca uses `self.wait(n)` con números hardcodeados. Usa siempre el `SyncManager`.
- **Abstracción**: No utilices primitivas de Manim (ej. `Tex`) directamente si existe un componente equivalente en la `Librería de Componentes` (ej. `MathText`).
## Workflow de Implementación (TDD Obligatorio)
1.  **Carga de Contexto**: Lee el archivo `meta.json` del Artefacto y el JSON de marcadores de audio (`cues.json`). Asegúrate de usar `pau_style.sty` para cualquier fragmento LaTeX.
2.  **Definición de Test (RED)**: Antes de escribir el código Manim, define un test (o script de validación) que verifique la existencia de los componentes esperados, la coherencia del `SyncManager` y que cualquier archivo `.tex` generado compile sin warnings.
3.  **Inicialización de Escena (GREEN)**: Todas las escenas deben heredar de `PAUScene`. Escribe el código mínimo para pasar el test, incluyendo la validación de compilación LaTeX.
...
4.  **Mapeo de Animaciones**:
...
    - Identifica cada marcador (`cue_id`) en el audio.
    - Asocia la animación correspondiente al marcador usando `self.sync.play_until("cue_id", animacion)`.
4.  **Layout y Composición**:
    - Usa el sistema de auto-encuadre de la librería.
    - Evita solapamientos usando `Mobject.next_to()` o las utilidades de bounding box de la librería.

## Referencias y Plantillas
- **Librería de Componentes**: Ver [COMPONENTS.md](./references/COMPONENTS.md) para la lista de clases disponibles.
- **Guía de Estilo Visual**: Ver [STYLE.md](./references/STYLE.md) para colores (CAM/PAU), tipografías y grosores de línea.
- **Plantilla Base**: Ver [template_scene.py](./assets/templates/template_scene.py).

## Triggers
- Se activa cuando se requiere traducir un Storyboard o un bloque de LaTeX en un script de animación ejecutable.
- Se activa al refactorizar animaciones existentes para mejorar la legibilidad o corregir errores visuales.
