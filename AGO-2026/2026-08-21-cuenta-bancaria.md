[Sesión - Cuenta Bancaria - 21 ago 2026](./sesiones/2026-08-21-cuenta-bancaria.md)

# Sesión - Cuenta Bancaria - 21 ago 2026

## Detalles de la sesión

| Proyecto | Qué hice | Por qué importa / qué aprendí |
| --- | --- | --- |
| dev-sandbox: Cuenta Bancaria | Implementé clase `BankAccount` con métodos `__init__`, `deposit()`, `withdraw()`, `get_balance()`, `display_info()`; constructor limpio con parámetros por defecto y validación de montos | Patrón de clase bien estructurada; validación transaccional |
| | Método `transfer(target_account, amount)` que ejecuta retiro + depósito condicionalmente usando booleano de retorno | Prevenir el "bug del dinero infinito" → ver detalle abajo |
| | Escenarios de prueba con 3 cuentas (Yarley, María, Carlos); simulador de cajero automático interactivo (comentado) | Verificación funcional; diferencia entre retorno de datos y UI → ver detalle abajo |

## Desarrollo y aprendizajes

#### Evitar efectos secundarios silenciosos en transacciones encadenadas (Bug del dinero infinito)

- **Contexto:** Implementar un método `transfer()` que reutilizara los métodos existentes `withdraw()` y `deposit()`.
- **Bloqueo:** En la primera versión, se ejecutaban `self.withdraw(amount)` y `target_account.deposit(amount)` de forma incondicional. Si el retiro fallaba por fondos insuficientes, el depósito se ejecutaba igualmente, creando dinero de la nada.
- **Solución y por qué:** Retornar un booleano en `withdraw()` (`True` si éxito, `False` si falla) e interceptarlo en `transfer()` mediante condicional `if`. Solo si el retiro retorna `True`, se procede al depósito. Esto preserva la reutilización de código (DRY), centraliza la lógica de validación en `withdraw()` y garantiza integridad transaccional.
- **Alcance:** Patrón de diseño transaccional reutilizable en cualquier lógica que encadene operaciones dependientes; establece una convención de seguridad aplicable a futuras extensiones.

#### Paso por referencia en métodos orientados a objetos

- **Contexto:** Entender cómo el método `transfer()` identifica y modifica la cuenta destino.
- **Bloqueo:** Confusión inicial sobre si era necesario buscar la cuenta destino por número de cuenta (`num_cuenta`) en lugar de manipular directamente la variable que almacena el objeto (`target_account`).
- **Detalle técnico:** En Python, los objetos se pasan por referencia. El parámetro `target_account` recibe la instancia completa en memoria. Cuando se invoca `target_account.deposit(amount)`, Python asigna automáticamente el objeto referenciado como argumento `self` de la función `deposit()`, permitiendo modificar el estado específico de esa instancia sin búsquedas manuales ni consultas a base de datos.
- **Solución y por qué:** Explicar el concepto mediante la analogía física: entregar el objeto directamente ("caja de datos") en lugar de enviar una dirección o número de referencia. Se visualizó agregando la instancia `carlos` al código de pruebas para ver la referencia en acción.
- **Alcance:** Concepto fundamental para la arquitectura orientada a objetos en Python; base para entender cómo se diseñan métodos que interactúan con otras instancias.

#### Retorno vs. Impresión de datos (return vs print)

- **Contexto:** Implementación de `get_balance()` para obtener el saldo de la cuenta.
- **Bloqueo:** En una iteración inicial, `get_balance()` imprimía directamente en terminal (`print(self.saldo)`) en lugar de retornar el valor.
- **Detalle técnico:** Si un método imprime en lugar de retornar, la información queda "atrapada" en la pantalla. El programa no puede almacenar ese valor en variables ni usarlo en comparaciones lógicas posteriores, limitando su reutilización.
- **Solución y por qué:** Usar `return self.saldo` para permitir la captura del dato en flujos lógicos complejos, como el simulador de cajero automático. Mantiene la separación entre funciones de negocio (que retornan datos) y funciones de interfaz (que imprimen).
- **Alcance:** Regla de diseño general en desarrollo: separar lógica de datos de lógica de presentación; fundamental para testing y reutilización.

## Enlaces y evidencia

- **Archivo modificado:** `cuenta_bancaria.py` en [dev-sandbox](https://github.com/devperez08/dev-sandbox) (ruta: `python/exercises/hard/cuenta_bancaria.py`)

## Habilidades aprendidas

- Diseño de clases con métodos de retorno booleano para control transaccional
- Paso de objetos por referencia en Python y cómo impacta el diseño de métodos
- Patrones de separación entre lógica de negocio e interfaz de usuario