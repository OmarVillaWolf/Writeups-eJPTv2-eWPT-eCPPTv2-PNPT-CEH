# Clases y Objetos 

Tags: #Python3 

**Clases**
Las clases son los fundamentos de la POO. Actúan como plantillas para la creación de objetos y definen atributos y comportamientos que los objetos creados a partir de ellas tendrán. En Python, una clase se define con la palabra clave ‘**class**‘ y proporciona la estructura inicial para todo objeto que se derive de ella.

**Instancias de Clase y Objetos**
Un objeto es una instancia de una clase. Cada vez que se crea un objeto, se está creando una instancia que tiene su propio espacio de memoria y conjunto de valores para los atributos definidos por su clase. Los objetos encapsulan datos y funciones juntos en una entidad discreta.

**Métodos de Clase**
Los métodos de clase son funciones que se definen dentro de una clase y solo pueden ser llamados por las instancias de esa clase. Estos métodos son el mecanismo principal para interactuar con los objetos, permitiéndoles realizar operaciones o acciones, modificar su estado o incluso interactuar con otros objetos.

```python 
#!/usr/bin/env python3 

class Persona:                                # Así creamos una clase 
	def __init__(self, nombre, edad):        # Así creamos un contructor (método)
		self.nombre = nombre
		self.edad = edad 

	def saludo(self):
		return f"Hola soy {self.nombre} y tengo {self.edad} años"

omar = Persona("Omar", 28)                    # Así creamos un objeto con sus atributos (Instancia)
print(omar.saludo())
```

```python 
#!/usr/bin/env python3 

class Animal:
	def __init__(self, nombre, animal):
		self.nombre = nombre
		self.animal = animal 

	def descripcion(self):
		print(f"{self.nombre} es un {self.animal}")

gato = Animal("Pipo", "Gato")
perro = Animal("Tobby", "Perro")

gato.descripcion()
perro.descripcion()
```

```python
#!/usr/bin/env python3

class CuentaBanc:
	def __init__(self, cuenta, nombre, dinero=0):       # Si no le pasas un valor a la var dinero, esta por defecto va a ser 0
		self.cuenta = cuenta 
		self.nombre = nombre
		self.dinero = dinero

	def Depositar(self, deposito):
		self.dinero += deposito
		return f"\n[+] [{self.nombre}] Se han depositado {deposito} dolares, actualmente tienes {self.dinero} dolares"

	def Retirar(self, retirar)
		if retirar > self.dinero:
			return f"\n[!] [{self.nombre}] Operación denegada: Fondos insuficientes\n"
		self.dinero -= retirar 
		return f"\n[+] [{self.nombre}] Se han retirado {retirar} dolares, actualmente tienes {self.dinero} dolares"

omar = CuentaBanc("123345", "Omar Villa", 1000)

print(omar.Depositar(500))
print(omar.Retirar(200))
```

**Decoradores**
Los decoradores son una herramienta poderosa en Python que permite modificar el comportamiento de una función o método. Funcionan como “envoltorios”, que agregan funcionalidad antes y después del método o función decorada, sin cambiar su código fuente. En POO, los decoradores son frecuentemente utilizados para agregar funcionalidades de manera dinámica a los métodos, como la sincronización de hilos, la memorización de resultados o la verificación de permisos.

**Métodos de Clase**
Un método de clase es un método que está ligado a la clase y no a una instancia de la clase. Esto significa que el método puede ser llamado sobre la clase misma, en lugar de sobre un objeto de la clase. Se definen utilizando el decorador ‘**@classmethod**‘ y su primer argumento es siempre una referencia a la clase, convencionalmente llamada ‘**cls**‘. Los métodos de clase son utilizados a menudo para definir métodos “factory” que pueden crear instancias de la clase de diferentes maneras.

**Métodos Estáticos**
Los métodos estáticos, definidos con el decorador ‘**@staticmethod**‘, no reciben una referencia implícita ni a la instancia (self) ni a la clase (cls). Son básicamente como funciones regulares, pero pertenecen al espacio de nombres de la clase. Son útiles cuando queremos realizar alguna funcionalidad que está relacionada con la clase, pero que no requiere acceder a la instancia o a los atributos de la clase.

```python 
#!/usr/bin/env python3 

class 
	def __init__(self, )


```