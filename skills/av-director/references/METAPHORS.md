# Catálogo de Metáforas Visuales (ai-manim-pau)

Este catálogo asegura que todos los vídeos hablen el mismo "lenguaje visual", ayudando al alumno a reconocer patrones matemáticos.

## Bloque: Álgebra Lineal
- **Matrices**: Siempre se presentan con `MatrixPAU`. Las operaciones (Suma, Multiplicación) deben mostrar el movimiento de las filas/columnas.
- **Rango**: Visualizar como la eliminación de filas dependientes (se desvanecen o se vuelven grises).
- **Determinantes**: Usar la metáfora de "área/volumen" para el significado geométrico y la "regla de Sarrus" con líneas de colores dinámicas.

## Bloque: Geometría
- **Vectores**: Flechas con etiquetas dinámicas. Usar `CoordinateSystemPAU`.
- **Planos**: Superficies semitransparentes. Las intersecciones deben resaltarse con líneas gruesas de color `ACCENT`.
- **Distancias**: Mostrar una línea de puntos que se estira y muestra el valor numérico.

## Bloque: Análisis (Cálculo)
- **Derivada**: Recta tangente que se desliza por la curva. Resaltar el crecimiento/decrecimiento con colores (Verde/Rojo).
- **Integral**: Llenado progresivo del área bajo la curva con rectángulos (Suma de Riemann) que colapsan en una superficie sólida.
- **Límites**: Acercamiento dinámico (zoom) a un punto con flechas que convergen.
