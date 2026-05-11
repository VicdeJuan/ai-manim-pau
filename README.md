# ai-manim-pau

Plataforma de automatización educativa para la preparación de la PAU de Matemáticas (2º de Bachillerato, Madrid).

## 🚀 Visión del Proyecto
**ai-manim-pau** combina el poder de la animación matemática con **Manim** y la inteligencia de agentes autónomos para ofrecer:
1.  **Generación de Contenido Audiovisual**: Producción automática de vídeos explicativos de alta calidad a partir de documentos LaTeX.
2.  **Corrección Inteligente**: Un pipeline de agentes capaz de transcribir ejercicios manuscritos y evaluarlos de forma justa mediante rúbricas estructuradas y gestión de "arrastre de error".

## 🛠️ Stack Tecnológico
- **Animación**: Manim Community (Estandarizado en [ADR 0001](docs/adr/0001-uso-de-manim-community.md)).
- **Orquestación**: LangGraph (Python) con persistencia en SQLite.
- **Modelos**: Gemini & Claude (vía Wrappers CLI).
- **Tipografía y Estilo**: LaTeX con estilo oficial del IES Francisco de Goya.

## 📂 Estructura del Proyecto
- `skills/`: Definición de habilidades para los agentes (Director AV, Manim Worker, Transcriptor, Calificador).
- `latex/`: Plantillas oficiales y recursos gráficos (`pau_style.sty`).
- `todos/`: Listado de tareas detallado por fases.
- `docs/`: Decisiones arquitectónicas (ADR) y documentación técnica.
- `dist/`: Skills empaquetadas listas para instalar.

## 📋 Desarrollo y Estándares
Este proyecto sigue normas estrictas para garantizar la calidad:
- **TDD Obligatorio**: Todo desarrollo debe comenzar con un test de comportamiento.
- **Validación LaTeX**: Todo documento `.tex` debe compilar sin warnings.
- **GitOps State**: El estado del sistema se gestiona mediante archivos sidecar `meta.json`.

## 📈 Roadmap
Estamos en la **Fase 1: Infraestructura Core**. Consulta el [PLAN.md](PLAN.md) para ver la hoja de ruta detallada y los próximos pasos.

---
*Desarrollado con ❤️ para ayudar a los estudiantes a superar la PAU.*
