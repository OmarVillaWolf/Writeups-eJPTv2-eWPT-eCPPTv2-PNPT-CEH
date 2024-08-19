# Encapsulamiento y métodos especiales 

Tags: #Python3 

El encapsulamiento en la programación orientada a objetos (POO) maneja principalmente tres niveles de visibilidad para los atributos y métodos de una clase: públicos, protegidos y privados. En Python, esta distinción se realiza mediante convenciones en la nomenclatura, más que a través de estrictas restricciones de acceso como en otros lenguajes.

**Atributos Públicos**
Son accesibles desde cualquier parte del programa y, por convención, no tienen un prefijo especial. Se espera que estos atributos sean parte de la interfaz permanente de la clase.

**Atributos Protegidos**
Se indica con un único guion bajo al principio del nombre (por ejemplo, ‘**_atributo**‘). Esto es solo una convención y no impide el acceso desde fuera de la clase, pero se entiende que estos atributos están protegidos y no deberían ser accesibles directamente, excepto dentro de la propia clase y en subclases.

**Atributos Privados**
En Python, los atributos privados se indican con un doble guion bajo al principio del nombre (por ejemplo, ‘**__atributo**‘). Esto activa un mecanismo de cambio de nombre conocido como ‘**name mangling**‘, donde el intérprete de Python altera internamente el nombre del atributo para hacer más difícil su acceso desde fuera de la clase.

**Métodos Especiales**
Los métodos especiales en Python son también conocidos como métodos mágicos y son identificados por doble guion bajo al inicio y al final (‘**__metodo__**‘). Permiten a las clases en Python emular el comportamiento de los tipos incorporados y responder a operadores específicos. Por ejemplo, el método ‘**__init__**‘ se utiliza para inicializar una nueva instancia de una clase, ‘**__str__**‘ se invoca para una representación en forma de cadena legible del objeto y ‘**__len__**‘ devuelve la longitud del objeto.

Algunos métodos especiales importantes en POO son:
- **__init__(self, […])**: Inicializa una nueva instancia de la clase.
- **__str__(self)**: Devuelve una representación en cadena de texto del objeto, invocado por la función ‘**str(object)**‘ y ‘**print**‘.
- **__repr__(self)**: Devuelve una representación del objeto que debería, en teoría, ser una expresión válida de Python, invocado por la función ‘**repr(object)**‘.
- **__eq__(self, other)**: Define el comportamiento del operador de igualdad ‘**==**‘.
- **__lt__(self, other), __le__(self, other), __gt__(self, other), __ge__(self, other)**: Definen el comportamiento de los operadores de comparación (**<**, **<=**, **>** y **>=** respectivamente).
- **__add__(self, other), __sub__(self, other), __mul__(self, other), etc.**: Definen el comportamiento de los operadores aritméticos (**+**, **–**, *****, etc.).

El encapsulamiento y los métodos especiales son herramientas poderosas que, cuando se utilizan correctamente, pueden mejorar la seguridad, la flexibilidad y la claridad en la construcción de aplicaciones. A lo largo de esta clase, exploraremos en detalle cómo implementar y utilizar estos conceptos y métodos para crear clases robustas y mantenibles en Python.


```python 

```