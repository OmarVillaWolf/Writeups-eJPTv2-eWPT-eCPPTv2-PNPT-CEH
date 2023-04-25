# Linux Commands ❮❯

Tags: #Linux #Comandos 

```bash
/tmp                                         # Directorios con capacidad de escritura en Linux
/var/tmp 
/dev/shm               
```

```bash
❯ nano /etc/hosts                            # Para agregar algun dominio existente
```

```bash
❯ ping -c 1 ❮IP❯                             # Para saber si la maquina esta activa o no

	# IP = IP Address de la maquina target 
	# c = Numero de pings a ejecutar
```

```bash
❯ whoami                                     # Miramos el nombre del usuario
```

```bash
❯ lsb_release -a                             # Miramos algunas caracteristicas de la maquina Linux 
```

```bash
❯ ip a                                       # Para ver las interfaces e IPs de una maquina
```

```bash
❯ ifconfig                                   # Para saber que IP hay en mi maquina 
```

```bash
❯ ifconfig docker0                           # Miramos la IP de la interfaz de Docker
```

```bash
❯ route -n                                   # Para mirar las routing tables
```

```bash
❯ hostname -I                                # Nos muestra solo la direccion IP
```

```bash
❯ arp-scan -I ens33 --localnet --ignoredups              # Para hacer un escaneo de la red 

	# I = i mayuscula; es la interface ens33
	# ignore = Ignora duplicados 
```


**Tip: Si nos dan una IP 192.168.11.2/24 con esa mascara que seria de clase C, podriamos hacer el escaneo con una mascara de clase B -> asi 192.168.0.0/16 en lugar de 192.168.11.0/24**
```bash
❯ masscan -p21,22,139,445,80,8080,443 -Pn ❮IP/16❯ --rate=5000    # Es una herramienta como nmap que sirve para aplicar escaneos a nivel de red, pero mas potente y rapida

	# Escanea millones de host por minuto
	# En un simple paquete te descubre todos los puertos abiertos que pueda tener un unico host
```

```bash
❯ exiftool ❮File❯                           # Para ver si hay metadatos en un archivo, imagen
```

```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80
```

```bash
❯ wget http://❮IP❯/❮File❯                   # Para poder cargar o descargar un archivo especifico desde una IP
```

```bash
❯ hashid <2b22337f218b2d82dfc3b6f77e7cb8ec> # Podemos saber el tipo de hash, no es muy confiable
```

```bash
❯ hash-identifier                           # Abriremos la tool y desoues colocaremos el hash a encontrar
	2b22337f218b2d82dfc3b6f77e7cb8ec

	# MD5 = Tiene 32 caracteres
```

```bash
❯ echo -n "2b22337f218b2d82dfc3b6f77e7cb8ec" | wc -c           # Nos muestra el numero de caracteres en una linea
```

```bash
❯ lsof -i:80                                # Para ver que servicio esta ocupando cierto puerto
```

```bash
❯ pwdx 1355887                              # Le pasamos el PID del comando anterior 'lsof' y asi podemos ver en que ruta se esta ejecutando ese servicio 
```

```bash
❯ tcpdump -i tun0 icmp -n                   # Nos ponemos en escucha en la interfaz i = Tun0 para trazas ICMP, n = No DNS
```

```bash
❯ git clone https://❮IP❯                    # Nos clonamos un repositorio de Github
```

```bash
❯ svn checkout https://❮IP❯                # Para clonar una subcarpeta de Github y en donde dice /tree/master quitarlo de la url y colocar /trunk y el resto de la url
```

```bash 
❯ cat file | xclip -sel clip               # Para copiarnos el output a la Clipboard
```

Para las monturas en Docker serian los siguientes comandos
```bash
❯ mount -t cifs //127.0.0.1/myshare /mnt/mounted -o username=null,password=null,domain=,rw # Para crear una montura en un dir especifico de nuestra maquina **'/mnt/mounted'**, lo que modifiques en la montura se hara en la maquina real

	# t = Tipo de montura 'cifs'
	# o = Para que no me pida la passwd
	# rw = Crear la montura con capacidad de lectura y escritura
```

```bash
❯ umount /mnt/mounted                      # Para eliminar la montura que esta en un dir especifico
```

```bash
❯ mount | grep home                        # Nos muestra desde donde hasta donde esta creada esa montura
```

```bash
❯ base64 -w 0 File.sh | xclip -set clip             # Codificamos el contenido de un archivo en base64 y lo copiamos en la clipboard
```

```bash
❯ echo kiufgaiuafgajlfufa98ag676a85g6ga7 | base64 -d > File.sh  # Decodeamos el archivo en base64 y lo colocamos en un archivo que se llamara File.sh
```

```bash
❯ echo $PATH                              # Nos damos cuenta que tiene un PATH muy pequeno, por lo que vamos a copiar el nuestro a la maquina victima y asi poder extenderlo

❯ export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin   # Faltan mas PATH pero es para hacer el ejemplo
```
