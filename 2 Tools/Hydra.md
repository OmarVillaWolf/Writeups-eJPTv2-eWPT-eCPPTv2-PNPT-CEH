# Hydra ❮❯

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


Hydra fuerza bruta FTP
```bash
❯ hydra -l omar -P /usr/share/wordlists/rockyou.txt ftp://❮IP❯ -t 15 # Haremos un ataque de fuerza bruta al puerto SSH, antes de completar el comando con soble TAB podemos ver la lista de diccionarios

	# l (ele) -> usuario que en este caso es  'omar'
	# L -> Indicas un archivo que disponga de usuarios 
	# P -> Ruta del diccionario o 'archivo que contiene passwds'
	# t -> Lanzar tareas en paralelo al mismo tiempo
```

Hydra fuerza bruta SSH
```bash
❯ hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://❮IP❯ -t 15 -V -s 2222 # Haremos un ataque de fuerza bruta al puerto SSH, antes de completar el comando con soble TAB podemos ver la lista de diccionarios

	# ssh -> Puerto al que vamos a atacar 22
	# IP -> Direccion de la maquina victima
	# P -> Ruta del diccionario o 'archivo que contiene passwds'
	# l (ele) -> usuario que en este caso es  'root'
	# t -> Lanzar tareas en paralelo al mismo tiempo
	# V -> Verbosity
	# s -> Port y es el puerto al que nos queremos conectar 
```

Hydra fuerza bruta SMB
```bash 
❯ hydra -L user.list -P password.list smb://<IP>        # Para ver si esos usuarios y passwd son validos en un servidor SMB

	# L = Ruta o archivo que contiene los usuarios
	# P = Ruta o archivo que contiene las passwd
```