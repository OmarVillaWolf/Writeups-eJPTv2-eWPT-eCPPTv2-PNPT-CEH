## Descubrimiento de equipos en la red local (ARP e ICMP) y Tips

Tags: #Enumeracion #Comandos 

```bash 
❯ nmap -sn ❮IP/24❯                             # Aplica un barrido con ping, para ver por trazas ICMP ver si el equipo esta activo
	# IP -> Direccion para hacer el barrido y debe terminar con **.0/24**

❯ arp-scan -I ens33 --localnet                 # Para hacer un escaneo de la red (I=i mayuscula interface) en la interface ens33
❯ arp-scan -I ens33 --localnet --ignoredups    # Para hacer un escaneo de la red (I=i mayuscula interface) en la interface ens33 y que no salgan duplicados 

# Tip: Si nos dan una IP 192.168.11.2/24 con esa mascara que seria de clase C, podriamos hacer el escaneo con una mascara de clase B -> asi 192.168.0.0/16 en lugar de 192.168.11.0/24

❯ masscan -p21,22,139,445,80,8080,443 -Pn ❮IP/16❯ --rate=5000   # Es una herramienta como nmap que sirve para aplicar escaneos a nivel de red, pero mas potente y rapida
	# Escanea millones de host por minuto
	# En un simple paquete te descubre todos los puertos abiertos que pueda tener un unico host
```