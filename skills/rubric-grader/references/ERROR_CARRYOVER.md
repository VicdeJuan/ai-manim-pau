# Manual de Gestión del Arrastre de Error (PAU Madrid)

El principio fundamental es: **Un mismo error solo se penaliza una vez.**

## Definición de Escenarios

### Escenario A: Independencia Matemática
Si el error cometido en el Paso 1 (ej. determinante mal calculado) convierte el Paso 2 (ej. cálculo de la matriz inversa) en algo trivialmente fácil o imposible, el evaluador debe juzgar si el alumno ha demostrado conocimiento del procedimiento del Paso 2.
- **Acción**: Si el procedimiento es correcto dada la premisa errónea, calificar con la puntuación máxima del criterio.

### Escenario B: Error de Arrastre vs. Nuevo Error
Si el alumno comete un error en el Paso 1 y, al llegar al Paso 2, comete **otro** error diferente (ej. signo mal cambiado), se debe penalizar el nuevo error según la rúbrica, pero ignorando la desviación heredada del Paso 1.

### Escenario C: El "Punto de Corte" (0 en el ejercicio)
Existen errores tan graves que invalidan cualquier arrastre posterior:
- Errores de bulto conceptual (ej. $\sqrt{a^2 + b^2} = a + b$).
- Operaciones imposibles (ej. dividir por cero o logaritmo de número negativo en el campo real).
- **Acción**: En estos casos, la rúbrica suele marcar un "Fallo Crítico" que pone un 0 en todo el ejercicio.

## Guía para el Agente Calificador
Al evaluar con arrastre de error, el agente debe:
1. "Simular" la resolución ideal partiendo del dato erróneo del alumno.
2. Comparar el desarrollo del alumno contra esa "resolución ideal alternativa".
3. Si coinciden, el alumno tiene el 100% de los puntos de ese criterio.
