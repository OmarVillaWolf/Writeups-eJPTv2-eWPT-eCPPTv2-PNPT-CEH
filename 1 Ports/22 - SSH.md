# SSH ❮❯

Tags: #SSH #Puerto #Comandos 

SSH es un protocolo de administración remota que permite a los usuarios **controlar** y **modificar** sus servidores remotos a través de Internet mediante un mecanismo de **autenticación seguro**. Como una alternativa más segura al protocolo **Telnet**, que transmite información sin cifrar, SSH utiliza **técnicas criptográficas** para garantizar que todas las comunicaciones hacia y desde el servidor remoto estén cifradas.

Si la versión del servidor SSH es “**OpenSSH 8.2p1 Ubuntu 4ubuntu0.5**“, podemos determinar que el sistema está ejecutando una distribución de Ubuntu. El número de versión “**4ubuntu0.5**” se refiere a la revisión específica del paquete de SSH en esa distribución de Ubuntu. A partir de esto, podemos identificar el **codename** de la distribución de Ubuntu, que en este caso sería “**Focal**” para Ubuntu 20.04.
Todas estas búsquedas las aplicamos sobre el siguiente dominio:
-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)


❯  **ssh ❮User❯@❮IP❯** Para conectarnos por ssh en el puerto default 22
❯  **ssh ❮User❯@❮IP❯ -p 2222** Para conectarnos por ssh
* p -> En un puerto especifico









Podemos buscar en searchsploit **ssh user enumeration** <7.7 un exploit en Python2
- Comando -> **python2 45939.py < VICTIM IP> < USER> 2/dev/null** Para confirmar si ese usuario existe en esa IP

- Comando -> **ssh < user>@< IP>** Para conectarnos por ssh
- Comando -> **sshpass -p <'PASSWORD'> ssh < USER>@< IP>** Conectarnos por ssh   
- Comando -> **ssh -i id_rsa < user>@< IP>** Nos conectamos por ssh teniendo un id_rsa con privilegio 600
- Comando -> **ssh < user>@localhost** Por lo que cuando tenemos un authorized_key podemos entrar por SSH sin proporcionar passwd en forma local 

- Comando -> **hydra -L user.list -P password.list ssh://10.129.42.197** Hacemos Bruteforce para ver que usuario y passwrod son validos


Cuando estamos en una **restricted bash** (rbash) podemos meter comandos en el ssh, esto si la restricted bash esta mal configurada:
- Comando -> **ssh < USER>@< IP> < COMMAND>** Podemos ejecuta un comando, y no nos cargara la restricted bash, y en este caso podemos hacer que nos de una bash. 
	- **bash** -> Podemos colocar bash como comando, podremos interactuar aunque no nos de una pseudo-consola. Pero podemos hacerle el tratamiento y nos dara una:
		- Tratamiento de la consola Linux cuando entras a una maquina victima
			 - **Script /dev/null -c bash** Nos sales que esto no se puede hacer, por lo que hacemos lo siguiente
			 - **Ctrl + z**
			 - **stty raw -echo; fg**
			 -  **reset xterm**
			- **export TERM=xterm** Para poder hacer Ctrl +c y Ctrl + l (l=ele)
			- **export SHELL=/bin/bash**Hacemos que shell ahora valga bash
