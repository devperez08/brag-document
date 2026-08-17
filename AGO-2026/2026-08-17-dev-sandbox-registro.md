[Sesión - dev-sandbox - 2026-08-17](./sesiones/2026-08-17-dev-sandbox-registro.md)

# Sesión - dev-sandbox - 2026-08-17

## Detalles de la sesión

| Proyecto | Qué hice | Por qué importa / qué aprendí |
| --- | --- | --- |
| dev-sandbox | Sistema de registro de estudiantes y cursos usando diccionarios y sets: inscripción automática, búsqueda de cursos compartidos, y catálogo global | Aprendí patrones eficientes para gestionar relaciones uno a muchos, garantizar unicidad, y consolidar colecciones anidadas → ver detalle abajo |

## Desarrollo y aprendizajes

#### Gestión de relaciones uno a muchos y unicidad con Diccionarios y Sets
- **Contexto:** Registrar estudiantes y sus respectivos cursos, asegurando que no se registren duplicados de materias y permitiendo búsquedas rápidas de cursos compartidos.
- **Bloqueo:** Evitar duplicación de materias en la inscripción de cada alumno sin recurrir a validaciones manuales costosas en listas.
- **Detalle técnico:** El uso de conjuntos (`set`) almacenados como valores en el diccionario de registros garantiza unicidad por definición — Python rechaza automáticamente elementos repetidos. Las operaciones de búsqueda y adición tienen tiempo promedio O(1). La intersección de sets usando el operador `&` permite encontrar cursos compartidos entre dos alumnos en una sola línea de código (`cursos1 & cursos2`), evitando bucles anidados.
- **Solución y por qué:** Implementé un diccionario con nombres de estudiantes como claves y conjuntos de cursos como valores. Para búsquedas de cursos en común, uso la intersección `&`. Esta estructura garantiza unicidad nativa y permite cálculos rápidos sin validaciones manuales.
- **Alcance:** Aprendizaje local y aplicable a futuros desarrollos de lógica con colecciones en Python.

#### Consolidación de colecciones anidadas de forma eficiente
- **Contexto:** Obtener un catálogo de cursos global a partir de las inscripciones de todos los estudiantes registrados en el sistema.
- **Bloqueo:** Unificar múltiples conjuntos de cursos almacenados en los valores de un diccionario sin crear duplicados y de manera eficiente.
- **Detalle técnico:** Usar `registro.values()` itera sobre los conjuntos de cursos de cada estudiante. El método `.update(curso)` del conjunto catálogo acumulador fusiona todos los conjuntos en uno solo, eliminando duplicados automáticamente por naturaleza de los sets.
- **Solución y por qué:** Implementé `catalogo.update(curso)` iterando sobre `registro.values()`. Esta es una solución más pythónica y directa que bucles anidados o conversiones a listas, y mantiene eficiencia tanto en tiempo como en espacio.
- **Alcance:** Patrón de diseño útil cuando se quiere consolidar conjuntos anidados — aplicable a lógicas futuras que requieran fusión de colecciones.

## Habilidades aprendidas
- Uso eficiente de conjuntos (`set`) para garantizar unicidad y operaciones rápidas
- Operador de intersección `&` para búsqueda de elementos compartidos entre colecciones
- Método `.update()` para consolidar múltiples conjuntos en uno solo

## Enlaces y evidencia
- [Código del ejercicio](https://github.com/devperez08/dev-sandbox/tree/main/python/exercises/medium/registro_estudiantes)