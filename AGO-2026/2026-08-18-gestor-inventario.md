[Sesión - Gestor de Inventario - 2026-08-18](./sesiones/2026-08-18-gestor-inventario.md)

# Sesión - Gestor de Inventario - 2026-08-18

## Detalles de la sesión

| Proyecto | Qué hice | Por qué importa / qué aprendí |
| --- | --- | --- |
| dev-sandbox | Refactoricé comparación de tipos en menú (string vs entero) y corregí invocación de método sin paréntesis (`lower()`) | Análisis de tipos y diferencia entre referencia e invocación de métodos |
| dev-sandbox | Implementé `agregar_producto()` con control de duplicados mediante bandera lógica | Patrón reutilizable para validaciones sobre colecciones completas → ver detalle abajo |
| dev-sandbox | Normalicé nombres de producto a minúsculas con validación insensible a mayúsculas | Garantiza unicidad de registros independiente de cómo el usuario ingresa el texto |
| dev-sandbox | Implementé `buscar_producto()` usando `enumerate()` para localizar por nombre | Búsqueda manual en tuplas con coincidencia exacta |
| dev-sandbox | Implementé `eliminar_producto()` con localización por índice y confirmación del usuario | Ciclo completo CRUD: crear, buscar, actualizar (sensibilidad a mayúsculas), eliminar |

## Desarrollo y aprendizajes

#### Patrón de Bandera para Decisiones Globales sobre Colecciones

- **Contexto:** Prevenir la adición de productos con nombres duplicados al inventario.
- **Bloqueo específico:** La lógica inicial tomaba la decisión de insertar o rechazar dentro del bucle `for`, inmediatamente tras la primera comparación. Esto resultaba en que, si el elemento no coincidía en el primer ítem de la lista, el programa lo agregaba como válido, permitiendo duplicados más adelante.
- **Detalle técnico del hallazgo:** Para tomar una decisión basada en la evaluación **completa** de una colección (validar que *ningún* elemento coincida), no se puede actuar en el bloque `else` del bucle directamente. Se necesita una variable bandera lógica externa (`existe = False`) que se modifique solo si ocurre la condición buscada. Tras finalizar el bucle, el estado final de la bandera refleja la "vista general" de la colección, permitiendo entonces tomar la decisión.
- **Solución y por qué:** Implementé una bandera lógica inicializada en `False`, que se establece en `True` al encontrar coincidencia, combinada con un `break` para optimizar el flujo (no recorrer el resto de la lista una vez hallado el duplicado). Solo después de que el bucle concluye se evalúa el estado de `existe` para decidir si insertar o mostrar un error.
- **Alcance:** Patrón reutilizable para validaciones personalizadas en listas de tuplas o estructuras complejas, especialmente cuando la lógica de rechazo requiere evaluar toda la colección antes de tomar una decisión.

#### Referenciación vs Invocación de Funciones y Métodos en Python

- **Contexto:** Normalizar los nombres de productos a minúsculas antes de guardarlos en el inventario.
- **Bloqueo específico:** Al listar el inventario, el nombre aparecía como un objeto tipo método (`<built-in method lower of str object ...>`) en lugar del texto transformado.
- **Detalle técnico del hallazgo:** En Python, usar el nombre de un método sin paréntesis (`objeto.metodo`) genera una **referencia** a la función misma, no su ejecución. Para ejecutar el método y obtener el valor de retorno, se debe usar la sintaxis de invocación con paréntesis (`objeto.metodo()`). Sin el paréntesis, se guarda la referencia al método, no el resultado de aplicarlo.
- **Solución y por qué:** Cambié `nombre.lower` a `nombre.lower()` para que Python evalúe el método y retorne el string convertido a minúsculas.
- **Alcance:** Aprendizaje conceptual fundamental y esencial de Python que aplica cada vez que se interactúa con métodos (strings, listas, diccionarios, etc.).

## Enlaces y evidencia

- Archivo modificado: [`gestor_inventario.py`](https://github.com/devperez08/dev-sandbox/blob/main/python/exercises/medium/gestor_inventario.py)

## Habilidades aprendidas

- Patrón de bandera para validaciones complejas sobre colecciones
- Distinción entre referencia e invocación de métodos en Python