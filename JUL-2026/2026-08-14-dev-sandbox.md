# Sesión - dev-sandbox - 2026-08-14

## Detalles de la sesión

| Proyecto | Qué hice | Por qué importa / qué aprendí |
| --- | --- | --- |
| dev-sandbox | Implementé conteo de vocales usando un diccionario y `.lower()` para insensibilidad a mayúsculas | Estructura clara para asociar contadores a cada vocal |
| dev-sandbox | Implementé conteo de palabras descartando conteo manual de espacios; usé `.split()` y `len()` | Evita errores con espacios múltiples o en extremos del texto |
| dev-sandbox | Transformé vocales a mayúsculas y consonantes a minúsculas, optimizando iteraciones | Análisis de complejidad algorítmica → ver detalle abajo |

## Desarrollo y aprendizajes

#### Optimización de iteraciones en reemplazo de caracteres
- **Contexto:** Reemplazar vocales por su versión en mayúscula dentro de una cadena previamente convertida a minúscula.
- **Bloqueo:** El enfoque inicial recorría cada carácter del texto completo (`for i in texto`), ejecutando `replace()` sobre el string en cada iteración. Para textos largos, esto causaba cientos o miles de ciclos innecesarios.
- **Detalle técnico del hallazgo:** Al iterar sobre las llaves del diccionario de vocales (un conjunto fijo de 5 elementos) en lugar de sobre todos los caracteres, el número de iteraciones se reduce drásticamente a un máximo de 5, independientemente del largo del texto del usuario. La complejidad de este enfoque es O(n) en lugar de O(n × 5).
- **Alternativas consideradas:** Iteración carácter por carácter (desechado por ineficiente).
- **Solución elegida y por qué:** Iterar sobre el diccionario `vocales` (5 llaves) y ejecutar `.replace()` en cada paso. Es altamente eficiente, reduce significativamente la complejidad del bucle, y produce código más legible y mantenible.
- **Alcance:** Aprendizaje local sobre manipulación de strings y diseño de algoritmos eficientes en Python; patrón aplicable a otros ejercicios de transformación de texto.

## Enlaces y evidencia

- Archivo modificado: [`analizador_texto.py`](https://github.com/devperez08/dev-sandbox/blob/main/python/exercises/exercise_2_analizador_texto.py)

## Habilidades aprendidas

- Optimización de algoritmos en Python mediante reducción de la complejidad iterativa
- Manipulación eficiente de strings usando diccionarios como estructura de control

## Stack técnico usado

- **Python nativo:** bucles `for`, diccionarios, métodos de strings (`.lower()`, `.split()`, `.replace()`)

---