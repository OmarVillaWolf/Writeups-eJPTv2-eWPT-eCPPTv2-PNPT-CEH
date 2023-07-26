# eJPTv2 TIPS

## Tools a usar 

* Nmap
* Dirbuster
* Metasploit
* SMB / SAMBA
* Nikto
* Crackmapexec
* Hydra
* Msfvenom
* Searchsploit

* Hashes 
	* Dumping hashes

* Pivoting

* Gestores 
	* Wordpress
	* Joomla

* Diccionarios
	* /usr/share/wordlists/rockyou.txt 
	* /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
	* /usr/share/metasploit-framework/data/wordlists/unix_users.txt
	* /usr/share/metasploit-framework/data/wordlists/common_users.txt

* Pasar archivos de Linux/Linux o Linux/Windows 

* Escalación de privilegios 
	* Linux 
		* Kernel 
		* CRON
		* Binarios SUID
	* Windows 
		* Kernel 
		* Bypassing UAC
		* Access Token Impersonation

# CEH TIPS PRACTICAL
## Tools a usar 

* 6 horas del examen practico 

* Nmap 
* Metasploit
* Wireshark - Manual y debemos de hacer las practicas que vienen ahí
* Tcpdump
* THM E-Learning:
	* Nmap
	* Red Team 
* THM Maquinas similares:
	* Basic Pentesting
	* Red Team Fundamentals 

## Capítulo 1

---
nmap [IP] - Aquí se hacer el Three Way Hanshake completo
nmap -sS  - Aquí no se hace el TWH completo

----
Activo - Tiempo Real
Pasivo - Etapas 

Exploit - Quien me lleva hacia el destino (Software)
Payload - Hace la acción (Software)
	Tipo 1: Local, cuando se ejecuta de forma local en el mismo sistema
	Tipo 2: Remoto, cuando son ejecutados en forma remota, el atacante debe tener acceso al sistema victima 
	Tipo 3: Cliente, requiere la intervención del usuario del lado del cliente y son asíncronos 

* Abrir un puerto de una IP que este cerrado - nc -nlvp 2030 



* Explotar el puerto 22 SSH con Metasploit 
	* > search ssh 
	* > auxiliary/scanner/ssh/ssh_login
	* Necesitamos dos archivos (USERPASS_FILE, PASS_FILE) y debemos de agregarlos al Metasploit
		* > set PASS_FILE /home/omar/Desktop/claves.txt
		* > set USER_FILE /home/omar/Desktop/usuarios.txt
	* Cuando encuentra la clave y el usuario, podemos ver con el siguiente comando si se estableció una sesión
		* > session -l             ->      Miramos las sesiones establecidas
	* Nos conectamos a la sesión, Metasploit intentara abrir la conexión y así podremos usar los comandos 
		* > sessions -i ID       ->     Colocamos el ID que en este caso es 1,



* Explotar el puerto 21 FTP con Metasploit 
	* > search ftp vsftpd 2.3.4
		* > use 0
		* > set RHOSTS {IP}



* Explotar el puerto 21 FTP con Metasploit 

Tarea: Vulnerar el servicio 139 netbios-ssn, 5900 vnc, 445 smb, 23 Telnet

---

Servicio de Windows 
* MSRPC - Enumerar ese servicio
* Microsoft-ds - Enumerar el servicio 
----
Mallware 

* Llave Simétrica .- Passwd simétrica - Texto plano 
* Llave Roamy .- Clave root o privilegiado
* Llave Crypto - Encriptado bajo protocolo de encriptación - SHA256

---
### Capítulo

```bash 
# Práctico 

Wireshark  

	❯ 
```

### Capítulo 9 Sniffer

* Practicar: Explotación - Malware - Sniffer

```bash 
# Practico Esto se hace en Wireshark
x.x.x.x -> Retrasmision a c.c.c.c - Indique el puerto:   
❯ tcp.options.eol y debemos ver el transmission Control Protocol, puerto origen y puerto destino
# Se le llama trafico .eol
# Es un tema de Sniffer
	1. wireshark # Colocaremos Wireshark en la interfaz correcta y capturamos el trafico 
	2. Filtramos el trafico y miramos el puerto o el servicio 
```

## Capitulo 12

```bash 
❯ www.pent.com                 # Indique el tipo de desarrollo y si es susceptible a un XXS

Podemos ver los modulos en el 'Inspector' de la pagina web
# En la parte de Ready tenemos informacion 
```

## Capitulo 13

```bash 
# Tools 
VeraCrypt     # Tool de encriptacion y desencriptacion de volumenes de disco
	1. Crear volumen  # Forma de enciptar
	2. Crear contenedor de archivos cifrado
	3. Volumen VeraCrypt normal
	4. Seleccionamos un archivo 
	5. Tipo de cifrado / Algoritmo SHA-256 o SHA-512
	6. Tamaño 
	7. Password que nos daran en el examen 

	1. Seleccionar archivo # Forma de desenciptar, siu sale el error de 'Code id file' buscamos el otro archivo en la carpeta 
	2. Montar 
	3. Password que nos daran en el examen 
	4. Desmontar al finalizar 


HashMyFiles  # Tool para calculo de integridad - HASH
1. Abrimos la ruta 
2. Cargamos todos los archivos y nos mostrara sus tipos de Hash
3. Comparamos los hash de la herramienta con los de la copia de seguridad 

CryptoForge  # Tool para desenciptar / Los archivos que tengan la extencion .cfe
1. Click derecho - Encryptar 

1. Click derecho - Desencriptar con la passwd que nos dan en el examen 
```