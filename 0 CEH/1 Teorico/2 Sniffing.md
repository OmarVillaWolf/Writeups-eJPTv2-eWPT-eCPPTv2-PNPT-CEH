# Sniffing 

Tags:  #CEH #Sniffing

## Técnicas activas 

```bash 
1. MAC flooding               # Inunda la tabla CAM del switch, manda falsas MAC address a toda la red y se puede hacer con el siguiente comando:

❯ macof -i <eth0> 
	# eth0 = Interfaz a usar para generar el ataque 

2. MAC spoofing/duplicating   

❯ macchanger -s <eth0>                            # Mirar la MAC de la tarjeta del dispositivo 
❯ macchanger --mac=00:20:91:da:af:91 <eth0>       # Cambiamos la MAC de la tarjeta del dispositivo
❯ macchanger -r <eth0>                            # Cambiamos la MAC de la tarjeta del dispositivo por una MAC aleatoria

3. ARP poisoning              # Se pueden hacer los siguientes ataques 'Packet Sniffing, Session hijacking, VoIP call tapping, Manipulating data, Man-in-the-middle attack, Data interception, Connection hijacking, Connection resetting, Stealing passwords Denial-of-service (DoS) attack' y se puede hacer con el siguiente comando:

❯ arpspoof -i <eth0> -t <IP1> <IP2>      # Obtenemos el trafico de la maquina victima y lo podemos capturar con Wireshark 
	# eth0 = Interfaz a usar para generar el ataque 
	# t = Target 
	# IP1 = La IP de la maquina victima 
	# IP2 = La IP existente a sustituir puede ser el 'Gateway' 
❯ echo 1 > /proc/sys/net/ipv4/ip_forward # Debemos cambiar el valor de '0' a '1' para que el ataque anterior no haga una DoS, cuando terminemos podemos regresar el valor a '0' 

4. DHCP starvation attack     # Falsifica MAC address para enviar paquetes DHCP query para recibir un pauqte DHCP completo con el fin de obtener las direccion IP disponibles y se puede hacer con las siguientes herramientas:

❯ Yersinia 
❯ dhcpStarvation.py 
❯ Hyenae
❯ dhcpstarv
❯ Gobbler 

4.1 Rogue DHCP Server attack  # Configurar un servidor falso con el fin de que el atacante decida la 'IP, Gateway y DNS'

5. Switch port stealing       # Falsifica paquetes ARP con la MAC del target como origen haciendo que el swith rediriga el trafico en el puerto equivocado 

6. ARP spoofing attack        # Falsificar la tabla ARP de la maquina o switch con el fin de que el atacante reciba el trafico 

6.1 ARP spoofing detection    # Podemos detectar el ataque con las siguientes herramientas:

❯ Wireshark 
❯ Capsa portable network analyzer 
❯ ARP antispoofer

7. DNS poisoning              # Existen 4 ataques diferentes de DNS 'Intranet DNS spoofing (Local network), DNS cache posoning, Proxy server DNS poisoning, Internet DNS spoofing (Remote network)' y se puede hacer con las siguientes herramientas:

❯ DerpNSSpoof
❯ Ettercap
❯ DNS poisoning tool 
❯ Evilgrade
❯ DNS spoof 
❯ DNS-poison
```