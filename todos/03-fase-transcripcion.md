# Todo: Fase 3 - Pipeline de Transcripción y Validación

Transforma imágenes de ejercicios manuscritos en LaTeX validado por el alumno antes de pasar a calificación.

- [ ] **3.1: Agente Transcriptor** (TDD)
  - Input: imagen (JPG/PNG) de la entrega del alumno.
  - Output: LaTeX normalizado + `confidence_score` (float 0.0–1.0).
  - Umbrales: `>0.85` pasa directo, `0.60–0.85` pasa con aviso, `<0.60` activa rechazo temprano.
  - El cálculo del score debe considerar: ambigüedad de caracteres y coherencia matemática estructural.
  - Referencia: `skills/math-transcriber/references/OCR_PITFALLS.md`.

- [ ] **3.2: Rechazo Temprano** (TDD)
  - Si `confidence_score < 0.60`: no abrir editor, devolver al alumno con motivo de rechazo.
  - Clasificar motivo: `LEGIBILIDAD_FISICA` (imagen borrosa, mal encuadre) o `AMBIGUEDAD_MATEMATICA` (escritura ilegible).
  - El flujo de rechazo debe escribir el motivo en el `meta.json` del artefacto.

- [ ] **3.3 [👤 TÚ]: Decisión WYSIWYG**
  - Evaluar MathLive vs MathQuill: comprobar compatibilidad con el LaTeX generado por el Agente Transcriptor.
  - Decidir método de integración (web component, iframe, react lib).

- [ ] **3.4: Integración Editor WYSIWYG**
  - Recibir LaTeX del agente e inyectarlo en el editor.
  - Generar LaTeX limpio y "humano-editable" (sin macros complejas innecesarias).
  - Capturar la confirmación definitiva del alumno y escribirla en `meta.json` con `status: TRANSCRIPCION_VALIDADA`.
