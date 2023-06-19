# CEH TIPS



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

