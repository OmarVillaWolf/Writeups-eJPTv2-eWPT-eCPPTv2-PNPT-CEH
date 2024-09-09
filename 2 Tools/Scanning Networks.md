# Network Scanning Tools

Tags:  #Scanning #Networks #Netdiscover #Hping3 

## Técnicas para descubrir hosts

```bash 
1. ARP Ping Scan 
2. UDP Ping Scan
3. ICMP Ping Scan (ICMP Echo Ping 'ICMP Echo Ping Sweep', ICMP Timestamp Ping, ICMP Address Mask Ping)
4. TCP Ping Scan (TCP SYN Ping, TCP ACK Ping)
5. IP Protocol Ping Scan
```

## Herramientas 

```bash
1. Nmap o Zenmap
2. Hping3 
3. Metasploit
4. NetScanTools Pro
5. Angry IP Scanner 
6. SolarWinds
```

```bash 
❯ hping3 -1 IP              # Ping ICMP
❯ hping3 -A IP -p 80        # Escaneo ACK en el puerto 80
❯ hping3 -2 IP -p 80        # Escaneo UDP en el puerto 80
❯ hping3 -s IP -p 80 --tcp-timestamp    # A Firewalls and Timestamps
❯ hping3 -8 50-60 -s IP -v              # Escaneo SYN en el puerto 50,60 
❯ hping3 -s IP -a IP -p 22 --flood      # Inundacion SYN a una victima 
```

```bash 
❯ netdiscover                # Descubrimiento de la red. Es una herramienta ruidosa 
```
