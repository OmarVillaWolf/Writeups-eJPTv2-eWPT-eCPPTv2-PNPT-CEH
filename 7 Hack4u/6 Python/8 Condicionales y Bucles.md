# Control de flujo (Condicionales y Bucles)

Tags: #Python3 

**Condicionales**
Los condicionales son estructuras de control que permiten ejecutar diferentes bloques de código dependiendo de si una o más condiciones son verdaderas o falsas. En Python, las declaraciones condicionales más comunes son ‘**if**‘, ‘**elif**‘ y ‘**else**‘.
- **if**: Evalúa si una condición es verdadera y, de ser así, ejecuta un bloque de código.
- **elif**: Abreviatura de “**else if**“, se utiliza para verificar múltiples expresiones sólo si las anteriores no son verdaderas.
- **else**: Captura cualquier caso que no haya sido capturado por las declaraciones ‘**if**‘ y ‘**elif**‘ anteriores.

**Bucles**
Los bucles permiten ejecutar un bloque de código repetidamente mientras una condición sea verdadera o para cada elemento en una secuencia. Los dos tipos principales de bucles que utilizamos en Python son ‘**for**‘ y ‘**while**‘.

- **for**: Se usa para iterar sobre una secuencia (como una lista, un diccionario, una tupla o un conjunto) y ejecutar un bloque de código para cada elemento de la secuencia.
- **while**: Ejecuta un bloque de código repetidamente mientras una condición específica se mantiene verdadera.

**Control de Flujo en Bucles**
Existen declaraciones de control de flujo que pueden modificar el comportamiento de los bucles, como ‘**break**‘, ‘**continue**‘ y ‘**pass**‘.

- **break**: Termina el bucle y pasa el control a la siguiente declaración fuera del bucle.
- **continue**: Omite el resto del código dentro del bucle y continúa con la siguiente iteración.
- **pass**: No hace nada, se utiliza como una declaración de relleno donde el código eventualmente irá, pero no ha sido escrito todavía.


```python
#!/usr/bin/env python3 

for i in range(5)                             # Imprimimos 5 numeros, desde el 0 hasta el 4 
	print(i)


names = ["Omar", "Juan", "Pedro", "Santi"]    # Imprimimos todos los nombres de la lista
for name in names:
	print(f"El nombre es {name}")


for i, name in enumerate(names):              # Enumerate siempre te devuelve (Posicion, Valor), por eso debes colocar dos variables
	print(f"Nombre [{i+1}]: {name}")
```

```python
#!/usr/bin/env python3 

i = 0
while i < 5:                                  # Imprimimos 5 numeros, desde el 0 hasta el 4 
	print(i)
	i += 1
```