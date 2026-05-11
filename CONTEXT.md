# Contexto del Proyecto: ai-manim-pau

Plataforma de aprendizaje online de matemáticas de 2º de Bachillerato (Madrid) para la preparación de la PAU, con alta automatización en la generación de contenidos y corrección de ejercicios.

## Glosario de Términos

### Dominio Educativo
- **PAU**: Prueba de Acceso a la Universidad (EvAU en Madrid).
- **2º de Bachillerato**: Segundo curso de la etapa post-obligatoria en España.
- **Rúbrica Estructurada**: Documento de configuración (JSON/YAML) asociado a cada ejercicio que define programáticamente los criterios de evaluación. Incluye instrucciones específicas para los agentes, la puntuación máxima por criterio y un catálogo de **Errores Comunes** con sus penalizaciones exactas (ej. error de cálculo -0.25) o condiciones de fallo automático (ej. $(a+b)^2 = a^2 + b^2$ implica un 0 en todo el ejercicio).
- **Nivel de Dificultad**: Escala de 4 niveles para la progresión de los ejercicios.

### Automatización Audiovisual
- **LaTeX Fuente**: Documento base generado por el humano y un agente que contiene la teoría o el ejercicio.
- **Librería de Componentes (Capa de Abstracción)**: Un conjunto de clases y funciones predefinidas (ej. `GraficoPAU`, `ExplicacionEcuacion`) construidas sobre Manim. Asegura coherencia visual, reduce la verbosidad y facilita la generación de código fiable por parte de los LLMs.
- **Código Manim (Generado)**: Script de Python escrito por el Agente Trabajador que instancia y orquesta los elementos de la Librería de Componentes para generar animaciones matemáticas. Se utilizará **Manim Community** como motor estándar (ver ADR 0001).
- **Agente Director (Audiovisual)**: Orquestador que divide el LaTeX en secciones modulares, establece las directrices creativas y narrativas (estilos, metáforas visuales) y coordina a otros agentes.
- **Agente Trabajador (Manim)**: Especialista que traduce las directrices creativas y el contenido LaTeX en código Manim funcional, ejecutando la implementación técnica y los detalles visuales finos.
- **Agente Productor AV**: Encargado de la locución (voiceover) y sincronización.
- **Agente Evaluador (Visión)**: Analiza el vídeo renderizado para dar feedback técnico y estético.

### Automatización de Corrección
- **Entrega**: Solución enviada por el alumno (posiblemente imagen/foto).
- **Filtro de Estructura**: Validación inicial de legibilidad, lógica y formato.
- **LaTeX de Transcripción**: Documento generado por la IA que representa la interpretación de la entrega del alumno.
- **Editor WYSIWYG Matemático**: Interfaz visual (tipo MathLive o MathQuill) que oculta el LaTeX subyacente. Permite al alumno revisar y editar la transcripción de su entrega sin necesidad de conocer sintaxis LaTeX.
- **Agente Calificador**: Evalúa el contenido según la rúbrica específica.
- **Feedback Personalizado**: Comentarios generados para el alumno sobre sus aciertos y errores.

## Normas de Desarrollo

### Test-Driven Development (TDD) Obligatorio
Todo desarrollo de funcionalidad en este proyecto **DEBE** seguir estrictamente la metodología **TDD** utilizando la `skill-tdd`. 
- **Flujo Vertical:** No se permiten "capas horizontales" de tests masivos. Se debe seguir el ciclo: 1 Test (RED) -> Implementación Mínima (GREEN) -> Refactor.
- **Pruebas de Comportamiento:** Los tests deben verificar el comportamiento a través de interfaces públicas, no detalles de implementación.
- **Validación Continua:** Ninguna funcionalidad se considera terminada sin sus correspondientes tests de comportamiento pasando.

### Desarrollo en LaTeX
- **Estilo Base:** Todos los documentos `.tex` deben importar el archivo de estilo oficial del proyecto (`pau_style.sty`).
- **Validación de Compilación:** Siempre que se genere o modifique un archivo `.tex`, este **DEBE** ser compilado (vía `pdflatex` o `latexmk`).
- **Cero Warnings:** No se aceptan documentos que generen warnings de compilación (ej. *Overfull hbox*, referencias perdidas). La ausencia de warnings es un requisito de finalización de tarea.

## Organización del Contenido y Estado

### Taxonomía de Directorios
El contenido se organiza en un árbol jerárquico estricto en el sistema de archivos:
`Bloque (ej. Álgebra)` -> `Tema (ej. Matrices)` -> `Subtema` -> `Tipo/Dificultad (Teoría, Ejemplos, Ejercicios Básicos, Medios, Difíciles, Ampliación)` -> `Artefacto`.

### El concepto de "Artefacto" (State Management)
La **fuente única de verdad (Single Source of Truth)** para el estado del dominio se gestiona mediante el enfoque de **Archivos Sidecar (GitOps)**. 
- Un **Artefacto** es el directorio terminal de la taxonomía.
- Contiene el `.tex` original, los scripts generados, los medios renderizados y un archivo de metadatos (`meta.json` o `context.md` con frontmatter) que registra de forma duradera el estado actual del proceso.
- Si un vídeo se subdivide modularmente, se crean subcarpetas de artefactos derivados dentro de la principal.
- **Visualización de Estado (Dashboard)**: Se requiere una herramienta o interfaz cómoda que lea **exclusivamente** estos archivos de metadatos para visualizar rápidamente en qué fase del pipeline se encuentra cada Artefacto (ej. "Pte. de Locución", "Pte. Revisión Humana", "Aprobado").

### Stack Tecnológico Base
- **Modelos Fundacionales (Wrappers CLI):** Se emplearán modelos en la nube (`gemini-acp`, `claude-acp`). **Nota técnica sobre "acp":** Este término hace referencia a la orquestación a través del CLI (ej. `gemini-cli`) aprovechando suscripciones de usuario final como si fueran una API, con el objetivo de evitar los costes directos de pago por token de las APIs tradicionales. Se enrutarán tareas al modelo más adecuado (ej. Gemini para contexto/visión, Claude para código). No se usarán modelos locales.
- **Orquestación de Agentes:** **LangGraph (Python)**. Se utilizará para modelar los flujos de trabajo como grafos de estado. Su **Checkpointing** (persistencia en SQLite) se considera un estado transitorio/efímero usado estrictamente para reanudar ejecuciones en curso o pausar para validación humana (`interrupt()`). Al finalizar una fase importante, LangGraph tiene la responsabilidad de escribir el estado duradero en el `meta.json` del Artefacto correspondiente.
- **Renderizado Visual:** Manim Community (ManimCE) + visor HTML/JS interactivo para validaciones.
- **Estado de Dominio:** Archivos Sidecar (GitOps) estructurados en directorios.

## Operaciones y Soporte (Post-MVP)
- **Sistema de Disputas (Fallback Humano):** Aunque el sistema de IA será hiper-robusto (minimizando errores evidentes), se incluirá un botón de "Reclamar corrección" que saca el ejercicio del pipeline automático y lo envía a una cola de revisión manual humana, protegiendo la confianza del alumno.

## Privacidad y Seguridad (Post-MVP)
- **Cumplimiento GDPR:** Pipeline de anonimización automático. Las entregas manuscritas de los alumnos pasarán por un filtro inicial que borra metadatos EXIF y emborrona nombres/información personal antes de guardarse en la plataforma.

## Distribución y Marketing (Post-MVP)
- **Reciclaje de Contenido Automático:** Al finalizar la generación del vídeo principal, un agente adicional tomará el `.tex` y el vídeo largo para: 1) Extraer un clip de 30s en formato vertical (9:16) para TikTok/Instagram Reels. 2) Redactar un artículo optimizado para SEO para el blog de la plataforma.

## Plataforma Web y Negocio (Post-MVP)
Aunque el foco inicial (MVP) es el motor de IA, el producto final requiere una infraestructura SaaS estándar:
- **Gestión de Usuarios:** Login, roles (Alumno, Administrador).
- **Modelo de Negocio:** Pasarela de pago, suscripciones (facturación).
- **Experiencia de Usuario (UX):** Panel de progreso del alumno, estadísticas, y un sistema de **Gamificación** para fomentar la retención y el avance a través de los niveles de dificultad.

## Tareas Pendientes / Backlog (Para futuras sesiones)
- **Generación de Rúbricas (Fase de Creación):** Diseñar el flujo de trabajo donde el humano (asistido por un agente especializado) genera la `Rúbrica Estructurada` (JSON/YAML) en el momento de crear el ejercicio. Se descarta el uso de RAG durante la evaluación para garantizar que la calificación sea estrictamente determinista y basada únicamente en la rúbrica estructurada preaprobada.

### Evolución Arquitectónica (Mejoras Futuras)
- **Comunicación Orientada a Eventos (Event-Driven)**: Aunque inicialmente la orquestación se basa en la lectura/escritura del estado de los Artefactos, se plantea como futura mejora implementar un sistema de colas de mensajes (ej. Redis/RabbitMQ) para desacoplar a los agentes y hacer la comunicación asíncrona más robusta.

### Flujo Audiovisual (Generación y Validación)
Para minimizar errores en la generación 3D y asegurar la calidad sin excesiva revisión manual, se aplica una jerarquía estricta de validación en múltiples capas:

1. **Generación LaTeX**: (Humano + Agente) crea el documento base.
2. **División Modular**: El Agente Director divide el contenido y establece directrices.
3. **Filtro 1: Sincronización por Audio (Audio-Driven)**: El Agente Productor AV genera primero la locución (MP3) junto con un payload de metadatos (JSON) que contiene un array de "Cues" o "Markers" (ej. `{"id": "cue_matrix_intro", "start_time": 1.2}`). Para evitar errores de cálculo por parte del LLM, la Librería de Componentes expone un `SyncManager` que lee este JSON. El Agente Trabajador utiliza referencias semánticas a estos marcadores (ej. `self.sync.play_until("cue_matrix_intro", animacion)`) en lugar de hardcodear tiempos numéricos, dictando así la duración exacta y el ritmo de las animaciones.
4. **Filtro 2: Restricciones Programáticas**: La Librería de Componentes fuerza el uso de cámaras predefinidas y auto-encuadre matemático para asegurar que todos los elementos sean visibles. Se genera un set de opciones "seguras".
5. **Filtro 3: Selección por VLM (Juez de Visión)**: Si existen múltiples opciones válidas del paso anterior, el Agente Evaluador (Visión) analiza renderizados de baja calidad y selecciona el más didáctico. (Decisión arquitectónica deliberada: Se confía en el VLM para la evaluación heurística de calidad didáctica por encima de la pura validación geométrica).
6. **Filtro 4: Storyboard Web (Humano)**: El resultado se presenta al humano como una previsualización interactiva en el navegador (exportación HTML/JS de Manim). El humano ajusta parámetros (cámara, ritmo) mediante un panel de control.
7. **Renderizado Final**: Tras la aprobación humana en el paso 6, el Agente Trabajador realiza el renderizado definitivo en MP4 de alta calidad.

### Flujo de Corrección
1. **Filtro Lógico/Estructural**: Validación inicial de la legibilidad de la imagen subida.
2. **Transcripción y Editor Visual**: La IA transcribe la imagen a LaTeX. Se implementa un mecanismo de **Rechazo Temprano**: si la confianza de la IA en la transcripción es inferior a un umbral predefinido, se rechaza la imagen y se pide al alumno que vuelva a subirla, evitando mostrar un editor con contenido incomprensible. Si la confianza es alta, este LaTeX se inyecta en un **Editor WYSIWYG Matemático** en el navegador.
3. **Validación del Alumno**: El alumno revisa visualmente la transcripción y corrige posibles errores de OCR usando el editor visual, confirmando su entrega definitiva.
4. **Orquestación y Evaluación por Rúbrica**: 
   - Si la entrega es un examen completo, un **Agente Director Principal** divide la entrega y asigna cada ejercicio a un **Agente Director de Ejercicio**.
   - La **Rúbrica Estructurada** se modela como un **Grafo Acíclico Dirigido (DAG)** de criterios, no como tareas estrictamente paralelas. Esto gestiona las dependencias lógicas (ej. la "Resolución" depende del "Planteamiento"). 
   - El Agente Director de Ejercicio orquesta a los Agentes Calificadores según este DAG. Cuando un criterio depende de otro anterior, el Calificador recibe el informe del paso previo. Se aplica estrictamente el principio de **No Doble Penalización ("arrastre de error")**: si el planteamiento es incorrecto pero la resolución del nuevo sistema (si es de dificultad análoga) es matemáticamente perfecta, no se restan puntos en el criterio de resolución.
5. **Informe Consolidado**: Los Agentes Directores unifican las calificaciones individuales y el Agente Director Principal genera el feedback personalizado final y la nota global.
