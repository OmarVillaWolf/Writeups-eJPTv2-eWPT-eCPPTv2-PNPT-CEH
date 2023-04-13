# Linux Commands ❮❯

Tags: #Linux #Comandos 

❯ **nano /etc/hosts** Para agregar algun dominio existente

❯ **ping -c 1 ❮IP❯** Para saber si la maquina esta activa o no
* IP -> IP Address de la maquina target 
* c -> Numero de pings a ejecutar

❯ **whoami** Miramos el nombre del usuario
❯ **lsb_release -a** Miramos algunas caracteristicas de la maquina Linux 


❯ **ifconfig**  Para saber que IIP hay en mi maquina 
❯ **route -n** Para mirar las routing tables
❯ **hostname -I** Nos muestra solo la direccion IP (I=i mayuscula)
❯ **ifconfig** Nos muestra las interfaces y las direcciones IP
❯ **arp-scan -I ens33 --localnet** Para hacer un escaneo de la red (I=i mayuscula interface) en la interface ens33

**Tip: Si nos dan una IP 192.168.11.2/24 con esa mascara que seria de clase C, podriamos hacer el escaneo con una mascara de clase B -> asi 192.168.0.0/16 en lugar de 192.168.11.0/24**
❯ **masscan -p21,22,139,445,80,8080,443 -Pn ❮IP/16❯ --rate=5000** Es una herramienta como nmap que sirve para aplicar escaneos a nivel de red, pero mas potente y rapida
* Escanea millones de host por minuto
* En un simple paquete te descubre todos los puertos abiertos que pueda tener un unico host

❯ **searchsploit ❮exploit_name❯** Para buscar un exploit
❯ **searchsploit -x ❮exploit_name❯** Para ver el contenido del exploit (x=examin)
❯ **searchsploit -m ❮exploit_name❯** Para descargarnos el exploit .py/.txt (m=move)

❯ **exiftool ❮File❯** Para ver si hay metadatos en un archivo, imagen

❯ **python3 -m http.server 80** Nos montamos un servidor http 80
❯ **lsof -i:80** Para ver que servicio esta ocuoando cierto puerto
❯ **pwdx 1355887** Le pasamos el PID del comando anterior y asi podemos ver en que ruta se esta ejecutando ese servicio 

❯ **nc -nlvp 443** Para ponernos en escucha por Netcat en espera de la revershell para Linux
❯ **rlwrap nc -nlvp 443** Para ponernos en escucha por Netcat en espera de la revershell para Windows 

❯ **tcpdump -i tun0 icmp -n** Nos ponemos en escucha en la interfaz Tun0 para ICMP

❯ **git clone \https://❮IP❯** Nos clonamos un repositorio de Github
❯ **svn checkout \https://❮IP❯** Para clonar una subcarpeta de Github y en donde dice **/tree/master** quitarlo de la url y colocar **/trunk** y el resto de la url

❯ **mount -t cifs //127.0.0.1/myshare /mnt/mounted -o username=null,password=null,domain=,rw** Para crear una montura en un dir especifico de nuestra maquina **'/mnt/mounted'**, lo que modifiques en la montura se hara en la maquina real
* t -> Tipo de montura 'cifs'
* o -> Para que no me pida la passwd
* rw -> Crear la montura con capacidad de lectura y escritura
❯ **umount /mnt/mounted** Para eliminar la montura que esta en un dir especifico



