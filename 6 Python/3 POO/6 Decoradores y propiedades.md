# Decoradores y Propiedades 

Tags: #Python3 

**Decoradores**
Los decoradores en Python son una forma elegante de modificar las funciones o métodos. Se utilizan para extender o alterar el comportamiento de la función sin cambiar su código fuente. Un decorador es en sí mismo una función que toma otra función como argumento y devuelve una nueva función que, opcionalmente, puede agregar alguna funcionalidad antes y después de la función original.

**Propiedades**
Las propiedades son un caso especial de decoradores que permiten a los desarrolladores añadir “**getter**“, “**setter**” y “**deleter**” a los atributos de una clase de manera elegante, controlando así el acceso y la modificación de los datos. En Python, esto se logra con el decorador ‘**@property**‘, que transforma un método para acceder a un atributo como si fuera un atributo público.

**Getters y Setters**
- El “**getter**” obtiene el valor de un atributo manteniendo el encapsulamiento y permitiendo que se ejecute una lógica adicional durante el acceso.
- El “**setter**” establece el valor de un atributo y puede incluir validación o procesamiento antes de que el cambio se refleje en el estado interno del objeto.
- El “**deleter**” puede ser utilizado para definir un comportamiento cuando un atributo es eliminado con del.

