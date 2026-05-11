# PRD: ai-manim-pau - MVP del Motor de Automatización Educativa

## Problem Statement
Los alumnos de 2º de Bachillerato en Madrid se enfrentan a una alta presión para preparar la PAU de matemáticas. Los recursos actuales suelen ser estáticos (PDFs) o vídeos genéricos que no se adaptan a su ritmo ni ofrecen una corrección personalizada e inmediata de sus propios ejercicios manuscritos, la cual es vital para identificar errores de razonamiento antes del examen oficial.

## Solution
Una plataforma que automatiza la creación de contenido audiovisual de alta calidad (vía Manim) y la corrección de ejercicios mediante un pipeline de agentes inteligentes. El sistema utiliza una jerarquía de validación humana en puntos críticos (storyboard, transcripción) para garantizar el rigor pedagógico, manteniendo una alta eficiencia mediante la orquestación de LLMs.

## User Stories

### Generación de Contenido (Profesor/Administrador)
1. Como creador de contenido, quiero generar vídeos educativos a partir de LaTeX, para que el proceso de producción sea escalable.
2. Como creador, quiero que el sistema genere automáticamente la locución y sincronice las animaciones, para reducir el tiempo de edición manual.
3. Como creador, quiero validar un storyboard interactivo antes del renderizado final, para asegurar que la explicación visual es correcta.
4. Como administrador, quiero definir rúbricas estructuradas en JSON, para que la evaluación sea objetiva y consistente.

### Corrección de Ejercicios (Alumno)
5. Como alumno, quiero subir una foto de mi ejercicio resuelto a mano, para recibir feedback inmediato.
6. Como alumno, quiero validar la transcripción que la IA hace de mi ejercicio, para corregir errores de lectura antes de la evaluación.
7. Como alumno, quiero recibir una calificación desglosada por criterios (planteamiento, resolución, etc.), para entender exactamente dónde he fallado.
8. Como alumno, quiero que el sistema detecte errores arrastrados sin penalizarme dos veces, para que mi nota refleje mi capacidad de resolución real.
9. Como alumno, quiero recibir feedback constructivo personalizado sobre mis errores comunes, para no repetirlos en la PAU.

## Implementation Decisions

### 1. Gestión de Estado y Orquestación
- **Single Source of Truth:** El estado del sistema reside en archivos sidecar `meta.json` dentro de la estructura de directorios del proyecto (GitOps).
- **Orquestación:** Se utiliza LangGraph para gestionar los flujos de trabajo de los agentes. El estado de LangGraph (SQLite) es puramente transitorio para gestionar pausas (`interrupt()`) y reanudaciones.

### 2. Motor Audiovisual
- **Framework:** Estandarización en **Manim Community**.
- **Sincronización:** Uso de un `SyncManager` que consume un payload JSON de marcadores generados por el Agente Productor AV.
- **Validación VLM:** Uso de modelos de visión para seleccionar la mejor composición visual entre varias opciones generadas programáticamente.

### 3. Motor de Corrección
- **Evaluación DAG:** La rúbrica se ejecuta como un Grafo Acíclico Dirigido. Los resultados de criterios anteriores alimentan a los posteriores para gestionar el "arrastre de error".
- **UX de Transcripción:** Implementación de un umbral de confianza para "Rechazo Temprano". Si la transcripción es de baja calidad, se solicita nueva captura en lugar de abrir el editor WYSIWYG.

### 4. Estructura de Datos
- Organización jerárquica en disco: `Bloque -> Tema -> Subtema -> Artefacto`.
- Los artefactos contienen todo el ciclo de vida del contenido: fuente, scripts, metadatos y medios finales.

## Testing Decisions
- **Unit Testing:** Los módulos core (SyncManager, Calculador de Rúbricas, Parsers de Metadatos) deben tener tests unitarios exhaustivos.
- **Integration Testing:** Se realizarán tests de flujo completo (E2E) simulando la subida de una imagen y verificando el DAG de corrección.
- **Visual Regression:** No se aplicará en el MVP debido a la naturaleza dinámica de Manim, confiando en el filtro del Agente Evaluador y la validación humana.

## Out of Scope (MVP)
- Sistema de usuarios y roles completo.
- Pasarela de pagos y suscripciones.
- Gamificación y estadísticas avanzadas del alumno.
- Generación automática de contenido para redes sociales (clips verticales).
- Sistema de disputas humano-in-the-loop (Post-MVP).

## Further Notes
- El uso de "acp" (Wrappers CLI) es fundamental para la viabilidad económica del proyecto durante el desarrollo inicial.
