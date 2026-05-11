# Componentes de la Librería ai-manim-pau

Usa estos componentes en lugar de las clases nativas de Manim siempre que sea posible.

## Módulos Core
- `PAUScene`: Clase base. Configura automáticamente la cámara, el fondo y el `SyncManager`.
- `SyncManager`: Gestiona el timing basado en `cues.json`. Métodos: `wait_for(cue_id)`, `play_until(cue_id, animation)`.

## Componentes Visuales
- `MathText(latex_string)`: Texto matemático con el estilo tipográfico oficial. Soporta coloreado automático de variables.
- `StepByStepExplanation(steps_list)`: Genera una lista vertical de pasos que se iluminan secuencialmente.
- `CoordinateSystemPAU()`: Plano cartesiano preconfigurado con ejes, rejilla y etiquetas estándar para 2º de Bachillerato.
- `MatrixPAU(matrix_data)`: Representación de matrices con brackets y espaciado optimizado.
- `GraphicBox(mobject)`: Envuelve un objeto en un marco destacado para énfasis.
