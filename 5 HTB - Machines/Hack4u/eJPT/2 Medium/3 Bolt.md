## Summary

- IP -> 10.10.11.114
- Ports -> TCP (22,80,443), UDP (idk)
- OS ->  Windows
- Services & Applications
    - 22 -> SSH Openssh 8.21p1 Ubuntu 4ubuntu0.3
    - 80 -> HTTP nginx 1.18.0
    - 443 -> HTTPS nginx 1.18.0


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
Openssh 8.21p1 4ubuntu0.3 Launchpad -> Focal
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
- Comando -> **whatweb http://< URL> -v**  (v=Miramos las cabeceras de la pagina Web) las cuales aveces nos revelan cosas

- Comando -> **openssl s_client -connect 10.10.11.114:443** Para verificar el certificado HTTPS, y ver si en los common names hay dominios y asi poderlos agregar 

Debemos de agregar el dominio que encontramos. passbolt.bolt.htb y bolt.htb

❯ **nmap --script http-enum -p80 10.10.11.114 -oN WebScan** 
-   -p80 -> Indica el puerto que se quiere escanear
-   10.10.11.130 -> Dirección IP que se quiere escanear
-   -oN WebScan -> Exporta el output a un fichero en formato nmap con nombre “WebScan”
-  http-enum -> Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

En la pagina Web entrando por el puerto 443 
Podemos hacer **Ctrl + u** para ver codigo de la pagina Web

Tambien podemos ver la pagina Web por el puerto 80 

Mirmamos que hay 3 usuarios y hacemos un diccionario con sus nombres en nano
joseph, bonnie, jose, jgarth, bgreen, jleos, j.garth, b.green, j.leos, neil, neilsims, nsims, n.sims

- Comando -> **impacket-GetNPUsers **

Ahi encontraremos un panel de autenticacion y en la pestana podemos observar que dice jinja

Buscaremos en **Google.com (PayloadAllTheThings -> Server Side Template Injection)**
Jinja2 -> Remote Code Ejecution 

En la pagina de descargas podemos descargar un archivo llamado 'image.tar'

- Comando -> **7.z l image.tar** (l=listar el contenido)
- Comando -> **7.z -xf image.tar** Descomprimimos el archivo 
- Comando -> **tree -fas** Para ver las rutas pero con toda su ruta 

- **tree -fas | grep 'layer.tar'** Para ver los archivos que contienen layer.tar
- **tree -fas | grep 'layer.tar' | awk 'NF{print $NF}'** Para que solo veamos el ultimo argumento 
- **for dile in $(tree -fas | grep 'layer.tar' | awk 'NF{print $NF}'); do echo -e "[+] Listando el contenido $file:\n"; 7z l $file; done | less -s** Ahora vamos a iterar para poder descomprimir cada uno de los archivos 

Despues de buscar en los archivos, en uno encontramos un db.sqlite3 en el cual podriamos encontrar informacion valiosa 
- **cd $(dirname ./34848468438846842684846842684/layer.tar)** Nos desplazamos a la ruta del archivo 
- **tar -xf layer.tar** Descomprimimos el archivo y ahi miramos el db.sqlite3 
- **file db.sqlite3** Miramos por los magic number si es sqlite3 y si lo es 

- **sqlite3 db.sqlite3** Nos conectamos a al archivo de sqlite3
	❯ **.tables** Para ver que tablas existen 
	❯ **select * from User;** Listamos todo el contenido que hay en esa tablay encontramos un usuario y una passwd que tendremos que romperla por fuerza bruta 

Hacemos un archivo llamado hash y ahi pegamos la passwd encriptada y lo trataremos de romper con jhon y el diccionario rockyou.txt
- Comando -> **john --wordlist=/usr/share/wordlists/rockyou.txt hash** Crackear un hash
	- Wordlist -> Ruta del diccionario 
	- Hashfile -> Archivo que contiene el hash
Con la passwd crackeada, podemos regresar al panel del login y ahi loggearnos con 'admin:deadbolt' y logramos entrar en la pagina.

Miramos la esquina de la pagina web y ahi podemos observar que dice **Flask** por lo que regresando a la pagina de Jinja2 podemos filtrar por flask y asi ver como podriamos acceder 

Ahora volvemos a mirar los dominios anteriores y entramos a uno por medio de https://passbolt.bolt.htb y firmamos el certificado 
Miramos que es una pagina diferente 



## User


## Root