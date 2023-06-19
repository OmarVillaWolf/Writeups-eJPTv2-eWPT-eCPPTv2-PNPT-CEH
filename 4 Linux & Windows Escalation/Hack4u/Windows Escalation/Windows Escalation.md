# Escalacion de privilegios Windows

Tags: #Windows #Escalada #Privilegios #NT-AuthoritySystem #WinPEAS #SystemInfo #NetUser #Whoami/Priv 

## Enumeracion Tools

* **Herramienta WinPEAS**: [WinPEAS](https://github.com/carlospolop/PEASS-ng/blob/master/winPEAS/winPEASexe/README.md) 
Nos dirigimos a: Download the **latest obfuscated and not obfuscated versions from here**, -> Despues debemos de descargar el archivo **winPeasx64.exe**

```bash
En WinPeas podemos encontrar las siguientes vulnerabilidades

	# Spoolsv
	# Checking AlwaysInstallElevated con las sig flags: 'set 1 in HKLM' y 'set 1 in HKCU'
```

## Manual 

```bash
❯  whoami /priv                              # Miramos los privilegios que tenemos   
	
	# SetImpersonatePrivilege = En el cual podriamos usar RottenPotato o  JuicyPotato 
	# SetLoadPrivilege = Para usar el recurso de Tarlogic

❯  whoami /all                               # Miramos todos los privilegios
```

Nos descargamos el de la siguiente pagina:
* [Juicy-Potato-Server-2003](https://binaryregion.wordpress.com/2021/08/04/privilege-escalation-windows-churrasco-exe/)

```bash
❯ systeminfo                                 # Nos copiamos todo lo que nos salga con ese comando y usaremos un programa llamado '' para deterctar vulnerabilidades en un equipo Windows, todo desde nuestra maquina Linux con el archivo que hemos creado con ese informacion obtenida.
```

```bash
❯ net user <USER>                            # Nos da detalles de nuestro usuario y vemos a que grupos pertenecemos. 


# Server Operators = Es un grupo en el que podemos administrar controladores de dominio, loggearse a un servicio interactivo, asi como crear, borrar recursos compartidos en la red, iniciar, parar servicios, back up, restaurar archivos, formatear el disco duro de la computadora y apagarla. Si pertenecemos a este grupo podemos cargar a la maquina victima Netcat.exe.

# 
```
