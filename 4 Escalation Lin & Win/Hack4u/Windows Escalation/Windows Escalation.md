# Escalación de privilegios Windows

Tags: #Windows #Escalada #Privilegios #NT-AuthoritySystem #WinPEAS #SystemInfo #NetUser #Whoami/Priv 

## Enumeración

* [WinPEAS](https://github.com/carlospolop/PEASS-ng/blob/master/winPEAS/winPEASexe/README.md)
Nos dirigimos a: Descargar el **latest obfuscated and not obfuscated versions from here**, -> Después descargar el archivo **winPeasx64.exe**

```bash
En WinPeas podemos encontrar las siguientes vulnerabilidades

	# Spoolsv
	# Checking AlwaysInstallElevated con las sig flags: 'set 1 in HKLM' y 'set 1 in HKCU'
```

## Manual 

```bash 
# Cuando tienes la sesion con Meterpreter 
❯ sysinfo                  # Muestra informacion del Windows 
❯ pgrep explorer           # Mirar el numero del proceso y asi poder escalar privilegios 
❯ pgrep lsass              # Mirar el numero del proceso y asi poder escalar privilegios 
❯ migrate <ID>             # Nos migramos al proceso 
 
❯ getuid                   # Miramos el nombre del usuario 
❯ getprivs                 # Miramos los privilegios 
❯ getsystem                # Miras los procesos privilegiados 

❯ getprivs                 # Miramos los  procesos con privilegios
```

```bash 
❯  getuid                                    # Nombre del servidor 
```

```bash
❯  whoami /priv                              # Miramos los privilegios que tenemos   
	
	# SetImpersonatePrivilege = En el cual podriamos usar RottenPotato o  JuicyPotato 
	# SetLoadPrivilege = Para usar el recurso de Tarlogic

❯  whoami /all                               # Miramos todos los privilegios
```

Nos descargamos el de la siguiente pagina:
* [Juicy-Potato-Server-2003](https://binaryregion.wordpress.com/2021/08/04/privilege-escalation-windows-churrasco-exe/)

Haremos lo siguiente si no encontramos alguna manera de escalar privilegios. 
```bash
❯ systeminfo                                 # Nos copiamos todo lo que nos salga con ese comando y usaremos un programa llamado 'Windows Exploit Suggester' para detectar vulnerabilidades en un equipo Windows, todo desde nuestra maquina Linux con el archivo que hemos creado con ese informacion obtenida.
```
* [Windows-exploit-suggester](https://github.com/AonCyberLabs/Windows-Exploit-Suggester)


```bash
❯ net user <USER>                            # Nos da detalles de nuestro usuario y vemos a que grupos pertenecemos. 


# Server Operators = Es un grupo en el que podemos administrar controladores de dominio, loggearse a un servicio interactivo, asi como crear, borrar recursos compartidos en la red, iniciar, parar servicios, back up, restaurar archivos, formatear el disco duro de la computadora y apagarla. Si pertenecemos a este grupo podemos cargar a la maquina victima Netcat.exe.
```
