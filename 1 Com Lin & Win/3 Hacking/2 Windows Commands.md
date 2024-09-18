# Windows Commands

Tags: #Windows #Comandos #Powershell 

## Comandos terminal Windows

* [WinPeas](https://github.com/peass-ng/PEASS-ng/blob/master/winPEAS/winPEASexe/README.md)

```bash
❯ ping -n 1 ❮IP❯                             # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

	# IP = IP Address de la maquina target 
	# n = Numero de pings a ejecutar
```

```bash 
❯ arp -a                                     # Mirar la tabla ARP en la maquina actual 
```

```bash 
❯ systeminfo                                 # Nos muestra la informacion de Windows (Parches 'Hotfix', etc...)
❯ sysinfo                                    # Nos muestra algunos detalles de Windows
```

```bash
❯ whoami                                     # Miramos el nombre del usuario
❯ whoami /priv                               # Miramos los privilegios 
```

```bash 
❯ query user                                 # Mirar si hay algun usuario loggeado
```

```bash
❯ net users                                  # Miramos todos los usuarios existentes y sus grupos
❯ net user <User>                            # Miramos el grupo de un usuario especifico como 'administrator'
❯ net localgroup administrators              # Miramos los miembros del grupo administrador
❯ net user admin password123                 # Para cambiar la passwd al usuario admin siendo NT Authority 
```

```bash 
❯ hostname                                   # Nombre de la maquina 
```

```bash
❯ .\file.exe               # Ejecutamos un arcxhivo .exe en Windows 
```

```bash 
❯ cd DOCUME~1              # Para ir a un dir que tenga espacios en su nombre 'Documents and settings', debemos de colocar las 6 primeras letras 
```

```bash
❯ cls                            # Nos ayuda a limpiar la pantalla
```

```bash
❯ ipconfig                       # Nos muestra las interfaces y las direcciones IP y si existen mas subredes, podemos hacer 'Pivoting'
```

```bash
❯ dir                            # Listamos el contenido del directorio
❯ dir /r /s ❮File.txt❯           # Buscamos de forma recursiva el string .txt
```

```bash
❯ type ❮File❯                                # Nos muestra el contenido del archivo
```


