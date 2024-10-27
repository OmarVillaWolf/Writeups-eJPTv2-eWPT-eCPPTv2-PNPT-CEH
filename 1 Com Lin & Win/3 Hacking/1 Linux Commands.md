# Linux Commands

Tags: #Linux #Comandos #Netcat #Montura #Hash 

## Banner Grabbing 

* Banner Grabbing: Es una técnica para reunir información usada por los pentesters para enumerar información del sistema operativo 'target', así, como los servicios que están corriendo en los puertos abiertos.  El objetivo primario es identificar el servicio que esta corriendo en un puerto especifico así como la versión del servicio. El banner grabbing se ejecuta a través de varias técnicas:
	* Ejecutar una detección de servicio con Nmap.
	* Conectarse a un puerto abierto con Netcat. 
	* Autenticarse con el servicio (Si el servicio soporta la autenticación) por ejemplo: SSH, FTP, Telnet, etc...

```bash 
1. Usar Nmap para ver los puertos abiertos, versiones, SO, banner, etc...
❯ nmap -sV --script=banner <IP>

2. Usar Netcat para conectarse a un puerto abierto, a veces nos muestra el 'Banner' 
❯ nc <IP> <Port>           

3. Usar Searchsploit para buscar algna vulnerabilidad en la version del puerto abierto
❯ searchsploit <servicio> <version>

4. Autenticarnos son el servicio, a veces nos muestra el 'Banner' 
❯ ssh <user>@<IP> 
```

## Comandos terminal Linux

* [LinPeas](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS)

```bash 
❯ apt update 
❯ apt upgrade 
❯ apt full-upgrade     

❯ parrot-upgrade
```

```bash
/tmp                                    # Directorios con capacidad de lectura y escritura en Linux       
/var/tmp 
/dev/shm      
/temp                                   # Directorios con capacidad de lectura y escritura en PHP

❯ Ctrl + u                              # Miramos el codigo fuente de la pagina 
❯ Ctrl + Shift + c                      # Abrimos el 'Inspect' de la pagina 
```

```bash
/etc/hosts                              # Para agregar algun dominio existente
/etc/passwd                             # Ruta de las cuentas de usuario
/etc/shadow                             # Hashes encriptados de passwds de los usuarios y solo puede acceder el usuario root 
/etc/group                              # Ruta de configuracion del archivo de grupos
/etc/shells                             # Podemos ver las diferentes shells que hay 
/etc/ssh/sshd_config                    # Podemos ver las configuraciones de ssh y ver si el usuario root se puede conectar 
```

```bash 
❯ Ctrl + k                              # Quitas todo lo que se encuentre a la derecha de donde estas posicionado

❯ kitty +kitten icat <image.jpg>        # Muestras la imagen por consola 
```

```bash
❯ echo ‘’ > ~/.zsh_history              # Para borrar el historial en la zshrc
```

```bash
❯ !$                                    # Referencias el ultimo argumento que hayamos puesto en la consola 
```

```bash 
❯ pushd /usr/share/                     # Para ir a un directorio especifico, funciona como el comando 'cd'
❯ popd                                  # Regresamos al directorio donde nos encontrabamos al momento de usar 'pushd'
```

```bash 
❯ scrub -p dod <file>                   # Primero haces esto y despues ejecutas el comando de abajo

❯ shred -zun 10 -v <file>               # Borrar un archivo, pero que el borrado sea mas complejo y asi no dejaremos evidencia, a mas valor que el 10 es mejor, ademas de borrarlo evita que se pueda recuperar
❯ shred -zun 10 -v file*                # Borrar todos los archivos que empiecen con una palabra en especifico
```

```bash 
❯ watch -n 1 ls -l /bin/bash            # Monitorizar la bash cada segundo con el siguiente comando y ver cuando cambia a SUID
```

```bash 
❯ extractPorts <filename>               # Herramienta que nos ayuda extraer los puerto de la captura de Nmap y copiarlos a la clipboard 
```

```bash
❯ ping -c 1 ❮IP❯                        # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

	# IP = IP Address de la maquina target 
	# c = Numero de pings a ejecutar

❯ ping -c 1 < TARGET IP> -R             # Verificar si tenemos conexion a una IP mandando numeros de ping especificos 
	
	# R = Traceroute, solo aplica en maquinas Linux
	# c = Numero de ping 

❯ fping -I ens33 -g <IP/24> -a 2>/dev/null   # Manda un ping a multiples IP
```

```bash
❯ ls -la                                # Podemos observar todos los archivos del dir inclyendo los ocultos 
```

```bash
❯ whoami                                # Miramos el nombre del usuario
```

```bash
❯ lsb_release -a                        # Miramos algunas caracteristicas de la maquina Linux 
```

```bash
❯ ip a                                  # Para ver las interfaces e IPs de una maquina
❯ ifconfig                              # Para saber que IP hay en mi maquina 
❯ ifconfig docker0                      # Miramos la IP de la interfaz de Docker
```

```bash
❯ file <FILE>                           # Nos muestra que tipo de archivo es por los magic numbers
```

```bash
❯ locate nc.exe                         # Buscar el Netcat para Windows
	❯ sudo updatedb                    # Actualizar la base de datos 
```

```bash
❯ find  / -name file.txt 2>/dev/null         # Para buscar un archivo en el sistema desde la raiz
```

```bash
❯ route -n                                   # Para mirar la tabla de ruteo
```

```bash
❯ netstat -nat                               # Para mirar la tabla de ruteo y algunos puertos abiertos

❯ netstat -tupln                             # Miras las IPs, los puertos, sus estados de proceso y su PID
	❯ kill -9  <PID>                        # Matamos un proceso especifico colocando el PID 

❯ netstat -antu                              # Miramos los estados de los puertos
	❯ kill -9 $(lsof -i:80 -t)              # Matamos un proceso especifico colocando el puerto 
```

```bash
❯ hostname -I                                # Nos muestra solo la direccion IP
```

```bash 
❯ ip a                                       # Miras la IP que tienes asignada 
❯ ifconfig                                   # Miras la IP asignada en Linux
```

```bash 
❯ netdiscover -i ens33 -r <IP/24>            # funciona enviando solicitudes ARP

	# i = Interface 
	# r = range 
```

```bash
❯ arp-scan -I ens33 --localnet --ignoredups       # Para hacer un escaneo de la red local, no puedes encontrar mas usuarios despues del router que no pertenecen a tu red

	# Escaneamos la Capa 3 del modelo OSI con el protocolo ARP el cual lo hacer por medio de Broadcast en un segmento de la red, y provee el MAC Address
	# I = i mayuscula; es la interface ens33
	# ignoredups = Ignora IPs duplicadas
	 
	# apt install arp-scan  = Instalar ARP-Scan
```

* [IP-Address-Calculator](https://www.ipaddressguide.com/cidr)

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
❯ fcrackzip -v -u -D -p /usr/share/worlists/rockyou.txt <File.zip>    # Nos ayuda a crackear un archivo zip con Fuerza Bruta

	# v = Verbose
	# u = Unzip
	# D = Dictionary attack
	# p = File to attack 'File.zip'
```

```bash
❯ scp <file> root@<IP>:/tmp/chisel          # Podemos tranferir un archivo y lo depositamos en la ruta que hayamos colocado
	# root = Usuario de la maquina victima 
	# IP = Direccion de la maquina victima 
	# file = Archivo a transferir 
```

```bash 
❯ md5sum file                             # Obtener el hash del archivo 
```

```bash
❯ wget http://❮IP❯/❮File❯                 # Para poder cargar o descargar un archivo especifico desde una IP de atacante
```

```bash
❯ curl http://❮IP❯:PORT                   # Para ver si hay contenido 
	# v = Verbosity
```

```bash
❯ echo -n "2b22337f218b2d82dfc3b6f77e7cb8ec" | wc -c           # Nos muestra el numero de caracteres en una fila
❯ cat File | wc -l                                             # Te muestra el numero de palabras en fila que existen en ese archivo (l=ele)
```

```bash 
❯ echo '50 40 73 73 77 30 72 64 40 ...' | xargs | xxd -ps -r   # Nos convierte la cadena de Hex a 

	# xargs = Nos pone la cadena en una sola linea
	# xxd = Nos invierte el proceso del Hex
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

```python
❯ 7z l <file.zip>                            # Podemos ver el contenido interno del archivo zip, gz, bzip2, etc... (l=ele)
❯ 7z x <file.gz>                             # Podemos extraer el contenido interno del archivo zip, gz, bzip2, etc...
❯ tar -xf <file.tar.gz>                      # Descomprimimos el archivo tar.gz
❯ gzip -d <file.txt.gz>                      # Descomprimir archivos gz
❯ unzip <file.zip>                           # Descomprimir un archivo zip 
❯ gunzip <file.gz>                           # Para descompirmir archivos gzip
```

```bash
# Para las monturas en Docker serian los siguientes comandos

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
❯ echo -n 'administrator@test.com' | base64      # Codeamos en base64 

❯ base64 data.zip                                # Codeamos en 'base64' el archivo data.zip, es una formade pasar un archivo desde la maquina victima a la de atacante 

	❯ nvim data.zip                             # Creamos el archivo en la maquina de atacante y agregamos el contenido del comando anterior 

❯ cat data.zip | base64 -d | sponge data.zip     # Decodeamos el contenido de 'data.zip' y con sponge agregamos el contenido decodeado al archivo mismo archivo 'sustitucion de contenido' 

❯ echo -n 'administrator@test.com' swaks         # Automatiza la creacion en base64
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
❯ du -hc <File>                           # Ver el peso del archivo 

❯ go build -ldflags "-s -w"               # Bajar el peso mientras se compila un archivo con 'go'
❯ upx <File>                              # Bajar el peso al archivo para transferirlo a la maquina victima mas rapido
```

```bash 
❯ php -f exploit.php                      # Ejecutar un archivo con extension PHP
```