# 1. Uso de Manim Community como Motor de Renderizado

Date: 2026-05-11

## Status

Accepted

## Context

El proyecto "ai-manim-pau" depende de un motor de animación matemática para renderizar los vídeos generados por los agentes (Agente Trabajador). La decisión principal recae sobre las dos bifurcaciones principales del proyecto Manim original de 3Blue1Brown: **Manim Community (ManimCE)** y **ManimGL**.

ManimGL está optimizado para la interactividad en tiempo real y la aceleración por hardware (OpenGL), lo que es ideal para uso humano en vivo. Sin embargo, nuestro pipeline está altamente automatizado y dirigido por LLMs. Requiere estabilidad de la API, ejecución *headless* fiable en servidores o procesos en segundo plano, y la capacidad de integrarse con herramientas web para la fase de "Storyboard Web" (HTML/JS).

## Decision

Estandarizaremos el uso de **Manim Community (ManimCE)** para todo el pipeline de generación audiovisual. 

## Consequences

- **Ventajas**:
  - **Ecosistema y Plugins**: Mayor soporte comunitario y plugins existentes que facilitan la exportación a formatos web o interactivos para el Storyboard Web.
  - **Estabilidad para LLMs**: La API de ManimCE está mejor documentada, lo que resulta en menor tasa de alucinaciones ("hallucinations") por parte de los agentes que generan el código de animación.
  - **Ejecución Headless**: Renderizado robusto en servidores de integración continua o flujos de trabajo sin interfaz gráfica.
- **Desventajas**:
  - Podría ser marginalmente más lento en el renderizado final comparado con la aceleración OpenGL de ManimGL, aunque esto se mitiga mediante la modularización de los vídeos en el pipeline.
