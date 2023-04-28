## Chisel 
#### Local Port Forwarding 
Nos permite redirigir todo el trafico entrante de un puerto local hacia un puerto remoto.
Server -> Windows 
Cliente -> Kali

Server -> **chisel.exe server -p < PUERTO DE ESCUCHA>**
Cliente -> **chisel.exe client < IP SERVER>:< PUERTO SERVER> < PUERTO LOCAL A ABRIR>:< IP REMOTA DEL PUERTO>:< PUERTO REMOTO QUE TRAEMOS>**

Comando -> **nmap -p< PUERTO LOCAL> 127.0.0.1**

#### Dinamic Port Forwarding 
Modificaremos el:
**nano /etc/proxychains.conf** Modificaremo este archivo en nuestra maquina
	socks4 127.0.0.1 1080 -> Haremos que la conexion para nuestra maquina se efectue en el puerto 1080

- Comando -> **ssh < user>@< IP> -D 1080** Para conectarnos por ssh (D=Dinamic Port Forwarding) y colocamos el puerto que anteriormente habiamos configurado.
- Comando -> **proxychains vncviewer -passwd secret 127.0.0.1:5901** Nos conectamos por vncviewer y nos arrojara el cuadro remoto de la conexion (5901=puerto remoto expuesto en la maquina victima)
	secret -> Archivo que contiene la passwd para la conexion
