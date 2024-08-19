# Herencia y polimorfismo 

Tags: #Python3 

La herencia y el polimorfismo son conceptos fundamentales en la programación orientada a objetos que permiten la creación de una estructura de clases flexible y reutilizable.

**Herencia**
Es un principio de la POO que permite a una clase heredar atributos y métodos de otra clase, conocida como su clase base o superclase. La herencia facilita la reutilización de código y la creación de una jerarquía de clases. Las subclases heredan las características de la superclase, lo que permite que se especialicen o modifiquen comportamientos existentes.

**Polimorfismo**
Este concepto se refiere a la habilidad de objetos de diferentes clases de ser tratados como instancias de una clase común. El polimorfismo permite que una función o método interactúe con objetos de diferentes clases y los trate como si fueran del mismo tipo, siempre y cuando compartan la misma interfaz o método. Esto significa que el mismo método puede comportarse de manera diferente en distintas clases, un concepto conocido como sobrecarga de métodos.

Ambos, la herencia y el polimorfismo, son piedras angulares de la POO y son ampliamente utilizados para diseñar sistemas que son fácilmente extensibles y mantenibles.

En esta clase, exploraremos cómo implementar herencia en Python y cómo se puede aprovechar el polimorfismo para escribir código más general y potente. Estos conceptos nos ayudarán a entender mejor cómo construir jerarquías de clases y cómo los diferentes objetos pueden interactuar entre sí de manera flexible.

## Herencia 

```python 
#!/usr/bin/env python3 

class Animal: 
	def __init__(self, nombre):        # Creamos el contructor 
		self.nombre = nombre 

	def hablar(self):                  # Creamos el metodo 
		pass                          # No hace nada y da a entender que debes de crear una subclase 

class Gato(Animal):                     # Creas una subclase  y ahi tenemmos la herencia 
	def hablar(self):
		return f"{self.nombre} dice Miau!"

class Perro(Animal):                    # Creas una subclase  
	def hablar(self):
		return f"{self.nombre} dice Guau!"	

gato = Gato("Firulais")
perro = Perro("Alfredo")

print(gato.hablar())
print(perro.hablar())
```

```python 
#!/usr/bin/env python3

class Autmovil:
	def __init__(self, marca, modelo):
		self.marca = marca
		self.modelo = modelo 

	def describir(self):
		return f"Vehiculo: {self.marca} {self.modelo}"

class Coche(Automovil):
	def describir(self):
		return f"Coche: {self.marca} {self.modelo}"

class Moto(Automovil):
	def describir(self):
		return f"Moto: {self.marca} {self.modelo}"

coche = Coche("Toyota", "Corolla")
moto = Moto("Honda", "CBR")

print(coche.describir())
print(moto.describir())
```

## Polimorfismo

```python 
#!/usr/bin/env python3 

class Animal: 
	def __init__(self, nombre):        # Creamos el contructor 
		self.nombre = nombre 

	def hablar(self):                  # Creamos el metodo 
		pass                          # No hace nada y da a entender que debes de crear una subclase 

class Gato(Animal):                     # Creas una subclase  y ahi tenemmos la herencia 
	def hablar(self):
		return f"Miau!"

class Perro(Animal):                    # Creas una subclase  
	def hablar(self):
		return f"Guau!"	

def hace_hablar(objeto):                # Creamos un metodo fuera de la clase, aqui tendriamos el polimorfismo 
	print(f"{objeto.nombre} dice {objeto.hablar()}")

gato = Gato("Firulais")
perro = Perro("Alfredo")

hacer_hablar(gato)
hacer_hablar(perro)
```

```python 
#!/usr/bin/env python3 

class Autmovil:
	def __init__(self, marca, modelo):
		self.marca = marca
		self.modelo = modelo 

	def describir(self):
		return f"Vehiculo: {self.marca} {self.modelo}"

class Coche(Automovil):
	def describir(self):
		return f"Coche: {self.marca} {self.modelo}"

class Moto(Automovil):
	def describir(self):
		return f"Moto: {self.marca} {self.modelo}"

def describir_vehiculo(vehiculo):         # Polimorfismo
	print(vehiculo.describir())

coche = Coche("Toyota", "Corolla")
moto = Moto("Honda", "CBR")

describir_vehiculo(coche)
describir_vehiculo(moto)
```