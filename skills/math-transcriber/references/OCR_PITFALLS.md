# Guía de Fallos Comunes de OCR y Ambigüedad Matemática

Para elevar el `confidence_score` y evitar el "Rechazo Temprano" innecesario, el transcriptor debe prestar atención a estas ambigüedades clásicas en los exámenes de la PAU.

## Ambigüedades de Caracteres
- **`2` vs `z`**: En álgebra (matrices), la `z` es común como incógnita. Si el trazo no tiene la barra central (ƶ), verificar el contexto de la ecuación.
- **`5` vs `s`**: Común en probabilidad o integrales.
- **`x` vs `×` (multiplicación)**: En 2º de Bachillerato, la multiplicación debe ser un punto `\cdot` o implícita. Si se detecta una `x` como operador, transcribir como `\cdot` y bajar levemente la confianza.
- **`0` vs `O` (matriz nula)**: El contexto de la operación de matrices determina si es el número cero o la matriz O.

## Estructuras Críticas
- **Matrices sin paréntesis/corchetes**: Si el alumno escribe una cuadrícula de números sin bordes, el OCR debe detectarla como matriz e informar de "Baja confianza estructural".
- **Líneas de fracción infinitas**: El OCR debe identificar claramente qué términos están sobre y bajo la línea para evitar errores de jerarquía en el calificador.
- **Exponentes vs Subíndices**: Especial atención a los elementos de una matriz $a_{ij}$. Un subíndice mal leído como exponente invalida toda la resolución.

## Criterios de Rechazo Temprano (Ejemplos)
- **Tachaduras masivas**: Si más del 30% del área con texto contiene tachones que ocultan símbolos, rechazar.
- **Escritura en vertical/diagonal**: Si el ángulo de la línea de base varía más de 20 grados, el riesgo de error de jerarquía es crítico.
