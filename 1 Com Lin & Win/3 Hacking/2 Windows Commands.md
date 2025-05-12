# Windows Commands

Tags: #Windows #Comandos 

## Comandos terminal Windows

```bash 
❯ del /s /q "file"                     # Eliminar un archivo 
❯ rmdir /s /q "C:/dir/"                # Eliminar una carpeta 
```

```bash
❯ ping -n 1 ❮IP❯                       # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

	# IP = IP Address de la maquina target 
	# n = Numero de pings a ejecutar
```

```bash 
❯ arp -a                               # Mirar la tabla ARP en la maquina actual 
```

```bash 
❯ systeminfo                           # Muestra la informacion de Windows (Parches 'Hotfix', etc...)
❯ sysinfo                              # Muestra algunos detalles de Windows
```

```bash
❯ whoami                               # Miramos el nombre del usuario
❯ whoami /priv                         # Miramos los privilegios 'Token' del usuario 
```

```bash 
❯ query user                           # Mirar si hay algun usuario loggeado
```

```bash
❯ net users                            # Miramos todos los usuarios existentes y sus grupos de forma local
❯ net user <User>                      # Miramos el grupo de un usuario especifico como 'administrator' de forma local
❯ net localgroup administrators        # Miramos los miembros del grupo administrador de forma local
❯ net user admin password123           # Para cambiar la passwd al usuario admin siendo NT Authority\System de forma local

❯ net user omar P4ssw0rd /add               # Crear un usuario siendo NT Authority\System de forma local
❯ net localgroup Administrators omar /add   # Agregar al usuario al grupo local 'Administrators'
```

```bash 
❯ hostname                 # Nombre de la maquina 
```

```bash
❯ ./SharpHound.exe         # Ejecutar un archivo .exe en Powershell o CMD
```

```bash 
❯ cd DOCUME~1              # Para ir a un dir que tenga espacios en su nombre 'Documents and settings', debemos de colocar las 6 primeras letras 
```

```bash
❯ cls                            # Limpia la pantalla
```

```bash
❯ ipconfig                       # Muestra las interfaces, direcciones IP y si existen mas subredes se puede hacer 'Pivoting'
```

```bash
❯ dir                            # Lista el contenido del directorio
❯ dir C:\Users                   # Lista los directorios de los usuarios  
❯ dir /r /s ❮File.txt❯           # Busca de forma recursiva el string .txt
```

```bash
❯ type ❮File❯                    # Muestra el contenido del archivo
```

```bash 
❯ route add IP mask 255.255.255.255 GW   # Agregar una IP en especifico
	# IP = Direccion a agregar
	# GW = Direccion IP del Gateway

❯ route add IP mask 255.255.255.0 GW     # Agregar un segmento de red 

❯ route delete IP                        # Borrar una IP especifica o un segmento de red
```