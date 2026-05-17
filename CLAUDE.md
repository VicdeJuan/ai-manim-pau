# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Contexto del proyecto

Lee `CONTEXT.md` antes de cualquier tarea de implementación. Contiene el glosario de dominio canónico, las normas de desarrollo obligatorias (TDD, LaTeX), la arquitectura de los dos pipelines y el stack tecnológico. Es la fuente de verdad del proyecto.

## Resumen rápido

Plataforma de automatización educativa para la PAU de Matemáticas. Dos pipelines independientes:
- **AV Pipeline**: LaTeX → agentes LangGraph → Manim Community → MP4, con revisión humana del storyboard.
- **Grading Pipeline**: imagen manuscrita → transcripción LaTeX → editor WYSIWYG → evaluación por rúbrica (DAG).

El estado del dominio vive en archivos `meta.json` sidecar (GitOps). LangGraph + SQLite es estado efímero (solo para `interrupt()`/reanudación).

## Reglas de desarrollo (resumen; ver detalles en CONTEXT.md)

- **TDD obligatorio**: usar `/tdd`. RED → GREEN → Refactor. Tests sobre interfaces públicas.
- **LaTeX**: todo `.tex` importa `latex/pau_style.sty`. Compilar y verificar cero warnings tras cada cambio: `latexmk -pdf -interaction=nonstopmode <file>.tex`.
- **Manim**: nunca `self.wait(n)` con literales — siempre `self.sync.play_until(cue_id, anim)`. Nunca primitivas Manim si existe equivalente en la Librería de Componentes.

## Navegación clave

| Documento | Contenido |
|---|---|
| `CONTEXT.md` | Glosario, normas, arquitectura completa de pipelines, stack |
| `PLAN.md` | Fases de implementación (tareas `[👤 TÚ]` = requieren humano) |
| `todos/` | Detalle de cada fase con criterios de aceptación |
| `docs/adr/` | Decisiones arquitectónicas (ej. ManimCE sobre ManimGL) |
| `docs/schemas/` | Esquemas JSON (`meta.json`, `cues.json`, rubric) |
| `skills/<name>/SKILL.md` | Workflow de cada agente |
| `skills/<name>/references/` | Material de referencia del agente |
