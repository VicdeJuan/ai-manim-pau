---
name: av-director
description: Orchestrates creative and structural flow of educational math videos. Use when modularizing LaTeX content into scenes, defining visual metaphors, or generating instructions for audiovisual production.
---

# Skill: AV Director (Director de Orquesta Audiovisual)

Esta skill define el flujo de trabajo para transformar contenido académico estático (LaTeX) en una narrativa audiovisual coherente y dinámica para la PAU.

## Responsabilidades
- **Modularización**: Dividir el contenido LaTeX en escenas de máximo 60-90 segundos para facilitar el renderizado y la revisión.
- **Narrativa Visual**: Decidir qué elementos deben ser animados (ej. "el determinante se expande") y cuáles deben permanecer estáticos.
- **Directrices de Audio**: Redactar el guion de locución (voiceover) con indicaciones de tono y énfasis.
- **Control de Calidad**: Validar que la transición entre escenas sea fluida.
## Workflow de Dirección (TDD Obligatorio)
1.  **Análisis de Entrada**:
    - Lee el documento LaTeX fuente (debe usar `pau_style.sty`).
    - Identifica los conceptos clave y los pasos de resolución.
2.  **Definición de Test de Estructura (RED)**: Crea una prueba que valide la fragmentación esperada, la presencia de metáforas visuales y que los fragmentos de guion/LaTeX compilen sin warnings.
3.  **Segmentación de Escenas (GREEN)**:
...
    - Crea un "Storyboard" conceptual que pase el test de estructura.
    - Cada escena debe tener un objetivo pedagógico único.
4.  **Definición de Metáforas**:
...
    - Selecciona componentes de la `Librería de Componentes`.
    - Especifica animaciones de alto nivel (ej. "usar transformación lineal para mostrar el cambio de base").
4.  **Generación de Instrucciones**:
    - Produce el guion para `skill-av-producer`.
    - Produce las directrices técnicas para `skill-manim-worker`.

## Referencias
- **Catálogo de Metáforas Visuales**: Ver [METAPHORS.md](./references/METAPHORS.md) para patrones visuales aprobados por bloque (Álgebra, Geometría, etc.).
- **Estándar de Guion**: Ver [SCRIPT_FORMAT.md](./references/SCRIPT_FORMAT.md).

## Triggers
- Se activa al recibir un nuevo archivo LaTeX de teoría o ejercicio aprobado para producción de vídeo.
- Se activa cuando un `Evaluador de Visión` rechaza una escena por falta de claridad narrativa.
