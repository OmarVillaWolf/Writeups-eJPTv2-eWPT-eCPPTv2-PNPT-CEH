# Linux Commands ❮❯

Tags: #Linux #Comandos #Netcat #Montura 

```bash
/tmp                                         # Directorios con capacidad de lectura y escritura en Linux                                      
/var/tmp 
/dev/shm      
/temp                                        # Directorios con capacidad de lectura y escritura en PHP
```

```bash
/etc/hosts                                   # Para agregar algun dominio existente
/etc/passwd                                  # Ruta de las cuentas de usuario
/etc/shadow                                  # Ruta de hashes de passwrod de usuario estan almacenadas ahi
/etc/group                                   # Ruta de configuracion del archivo de grupos
/etc/shells                                  # Podemos ver las diferentes shells que hay 
/etc/ssh/sshd_config                         # Podemos ver las configuraciones de ssh y ver si el usuario root se puede conectar 
```

```bash
❯ echo ‘’ > ~/.zsh_history                   # Para borrar el historial en la zshrc
```

```bash
❯ !$                                         # Referencias el ultimo argumento que hayamos puesto en la consola 
```

```bash
❯ ping -c 1 ❮IP❯                             # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

	# IP = IP Address de la maquina target 
	# c = Numero de pings a ejecutar

❯ ping -c 1 < TARGET IP> -R                 # Verificar si tenemos conexion a una IP mandando numeros de ping especificos 
	
	# R = Traceroute, solo aplica en maquinas Linux
	# c = Numero de ping 
```

```bash
❯ ls -la                                     # Podemos observar todos los archivos del dir inclyendo los ocultos 
```

```bash
❯ whoami                                     # Miramos el nombre del usuario
```

```bash
❯ lsb_release -a                             # Miramos algunas caracteristicas de la maquina Linux 
```

```bash
❯ ip a                                       # Para ver las interfaces e IPs de una maquina
❯ ifconfig                                   # Para saber que IP hay en mi maquina 
❯ ifconfig docker0                           # Miramos la IP de la interfaz de Docker
```

```bash
❯ file <FILE>                                # Nos muestra que tipo de archivo es por los magic numbers
```

```bash
❯ locate nc.exe                              # Buscar el Netcat para Windows
```

```bash
❯ find  / -name file.txt 2>/dev/null         # Para buscar un archivo en el sistema desde la raiz
```

```bash
❯ route -n                                   # Para mirar la tabla de ruteo
```

```bash
❯ netstat -nat                               # Para mirar la tabla de ruteo y algunos puertos abiertos
```

```bash
❯ hostname -I                                # Nos muestra solo la direccion IP
```

```bash
❯ arp-scan -I ens33 --localnet --ignoredups              # Para hacer un escaneo de la red 

	# I = i mayuscula; es la interface ens33
	# ignoredups = Ignora IPs duplicadas 
```


**Tip: Si nos dan una IP 192.168.11.2/24 con esa mascara que seria de clase C, podriamos hacer el escaneo con una mascara de clase B -> asi 192.168.0.0/16 en lugar de 192.168.11.0/24**
```bash
❯ masscan -p21,22,139,445,80,8080,443 -Pn ❮IP/16❯ --rate=5000    # Es una herramienta como nmap que sirve para aplicar escaneos a nivel de red, pero mas potente y rapida

	# Escanea millones de host por minuto
	# En un simple paquete te descubre todos los puertos abiertos que pueda tener un unico host
```

```bash
❯ exiftool ❮Image.jpg❯                      # Para ver si hay metadatos en un archivo, imagen
❯ steghide info <Image.jpg>                 # Para ver si la imagen tiene contenido oculto
❯ steghide extract -sf <Image.jpg>          # Para ver si la imagen tiene contenido oculto (sf=sourcefile)
```

```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80
```

```bash
❯ wget http://❮IP❯/❮File❯                   # Para poder cargar o descargar un archivo especifico desde una IP
```

```bash
❯ hashid <2b22337f218b2d82dfc3b6f77e7cb8ec> # Podemos saber el tipo de hash, no es muy confiable

❯ hash-identifier                           # Abriremos la tool y desoues colocaremos el hash a encontrar
	2b22337f218b2d82dfc3b6f77e7cb8ec

	# MD5 = Tiene 32 caracteres
```

```bash
❯ echo -n "2b22337f218b2d82dfc3b6f77e7cb8ec" | wc -c           # Nos muestra el numero de caracteres en una fila
❯ cat File | wc -l                                             # Te muestra el numero de palabras en fila que existen en ese archivo (l=ele)
```

```bash
❯ lsof -i:80                                # Para ver que servicio esta ocupando cierto puerto
❯ pwdx 1355887                              # Le pasamos el PID del comando anterior 'lsof' y asi podemos ver en que ruta se esta ejecutando ese servicio 
```

```bash
❯ tcpdump -i tun0 icmp -n                   # Nos ponemos en escucha en la interfaz i = Tun0 para trazas ICMP, n = No trazas DNS
```

```bash
❯ git clone https://❮IP❯                    # Nos clonamos un repositorio de Github
❯ svn checkout https://❮IP❯                 # Para clonar una subcarpeta de Github y en donde dice /tree/master quitarlo de la url y colocar /trunk y el resto de la url
```

```bash 
❯ cat file | xclip -sel clip                # Para copiarnos el output a la Clipboard
```

```bash
❯ 7z l file.zip                              # Podemos ver el contenido interno del archivo zip, gz, bzip2, etc... (l=ele)
❯ 7z x file.gz                               # Podemos extraer el contenido interno del archivo zip, gz, bzip2, etc...
```

Para las monturas en Docker serian los siguientes comandos
```bash
❯ mount -t cifs //127.0.0.1/myshare /mnt/mounted -o username=null,password=null,domain=,rw # Para crear una montura en un dir especifico de nuestra maquina **'/mnt/mounted'**, lo que modifiques en la montura se hara en la maquina real

	# t = Tipo de montura 'cifs'
	# o = Para que no me pida la passwd
	# rw = Crear la montura con capacidad de lectura y escritura

❯ umount /mnt/mounted                      # Para eliminar la montura que esta en un dir especifico
❯ mount | grep home                        # Nos muestra desde donde hasta donde esta creada esa montura
```

```bash
❯ cp /bin/bash .                           # Copiamos la bash en el dir actual
```

```bash
❯ base64 -w 0 File.sh | xclip -set clip             # Codificamos el contenido de un archivo en base64 y lo copiamos en la clipboard
❯ echo kiufgaiuafgajlfufa98ag676a85g6ga7 | base64 -d > File.sh  # Decodeamos el archivo en base64 y lo colocamos en un archivo que se llamara File.sh
```

```bash
❯ echo $PATH                              # Nos damos cuenta que tiene un PATH muy pequeno, por lo que vamos a copiar el nuestro a la maquina victima y asi poder extenderlo
❯ export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin   # Faltan mas PATH pero es para hacer el ejemplo
```

```bash
❯ chown root:root bash                    # Le asignamos de propietario y grupo 'root' a la bash
❯ chown <USER:USER> -R <DIR_PATH>         # Le cambiamos el propietario a un directorio (R=Forma recursiva)
```

```bash
❯ chmod u+x <FILE>                        # Le cambiamos los permisos a un archivo (r=read, w=write, x=execution, u=user, g=grupo, o=other)
❯ chmod 4755 bash                         # Le cambiamos los privilegios SUID y ahora tendra una 's'
❯ chmod 600 id_rsa                        # Para darle un permiso 600 a la id_rsa   
```

```bash
❯ upx <FILE>                              # Podemos bajarle el peso al archivo para transferirlo a la maquina victima mas rapido
```


---

Instalar pip2 en Parrot
```python
$ wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
$ sudo python2.7 get-pip.py
```