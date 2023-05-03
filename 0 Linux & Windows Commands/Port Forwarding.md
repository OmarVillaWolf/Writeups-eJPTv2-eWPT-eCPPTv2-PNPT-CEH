## Chisel 

Tags: #Chisel #PortForwarding #LocalForwarding #DinamicForwarding 

#### Local Port Forwarding 
Nos permite redirigir todo el trafico entrante de un puerto local hacia un puerto remoto.
Server -> Windows 
Cliente -> Kali

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
❯ ./chisel server --reverse -p <Port-Server>          # Server 'Maquina de atacante'
❯ ./chisel client <IP-Server>:<Port-Server> R:<Port-Local-a-Abrir>:<IP-Local>:<Port-Traemos> # Cliente 'Maquina victima'

	# R = Remote Port Forwarding
	# IP-Server = IP de la maquina atacante
	# Port-Server = Puerto de la maquina atacante
	# Port-Local = Puerto que vamos a abrir en nuestra maquina 
	# IP-Local = IP del Localhost de la maquina victima 
	# Port-Traemos = Puerto que vamos a traer de la maquina victima 
```


#### Dinamic Port Forwarding 

Modificaremos:
```bash
❯ nano /etc/proxychains.conf                            # Modificaremo este archivo en nuestra maquina
	socks4 127.0.0.1 1080                              # Haremos que la conexion para nuestra maquina se efectue en el puerto 1080
```

```bash
❯ ssh < user>@< IP> -D 1080                             # Para conectarnos por ssh (D=Dinamic Port Forwarding) y colocamos el puerto que anteriormente habiamos configurado.
❯ proxychains vncviewer -passwd secret 127.0.0.1:5901   # Nos conectamos por vncviewer y nos arrojara el cuadro remoto de la conexion (5901=puerto remoto expuesto en la maquina victima)

	# secret -> Archivo que contiene la passwd para la conexion
```