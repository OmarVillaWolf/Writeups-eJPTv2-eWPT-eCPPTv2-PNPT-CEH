# Wireshark 

Tags: #Wireshark 

```bash
❯ wireshark &> /dev/null & disown                 # Ejecutamos el Wireshark y lo independizamos para seguir ocupando la terminal 
❯ wireshark ❮File.cap❯ &> /dev/null & disown      # Le pasamos la captura .cap y la abrimos en Wireshark
```

En el Software de Wireshark podemos:
```bash
❯ tcp                               # Filtrar por el protocolo 'TCP'
❯ tcp.port == 22                    # Filtrar por un puerto especifico, en este caso es el 22
❯ ip.dst == 192.168.68.111          # Filtramos los paquetes por esa IP destino 
❯ ip.flags.mf == 1                  # Forma de filtrar por paquetes fragmentados
❯ ip.flags.mf == 0                  # Forma de filtrar por paquetes que no son fragmentados
```

También podemos usar Tshark 
```bash
❯ tshark -r ❮File.cap❯ 2>/dev/null                         # Le pasamos la captura en .cap y la podemos mirar por consola 
❯ tshark -r ❮File.cap❯ -Y "http" 2>/dev/null               # Le pasamos la captura en .cap y la podemos mirar por consola (Y=Filtramos)
❯ tshark -r ❮File.cap❯ -Y "http" 2>/dev/null | grep "GET"  # Le pasamos la captura en .cap y la podemos mirar por consola (Y=Filtramos) y volvemos a filrar por el metodo GET
❯ tshark -r ❮File.cap❯ -Y "http" -Tjson 2>/dev/null        # Le pasamos la captura en .cap y la podemos mirar por consola (Y=Filtramos) y miramos los atributos de los paquetes por el formato Json 
❯ tshark -r ❮File.cap❯ -Y "http" -Tfileds -e tcp.payload 2>/dev/null | xxd -ps -r | grep "GET" | awk '{print $2}'  # Le pasamos la captura en .cap y la podemos mirar por consola (Y=Filtramos), filtramos por un campo especifico y nos lo sacara en Hexadecimal pero lo revertimos con xxd, volvemos a filtrar por GET y todo esto es para, ver la rutas que esta probando el script de la captura de http-enum
```

## Tcpdump

```bash
❯ tcpdump -i tun0 icmp -n                   # Nos ponemos en escucha 
	# i = Tun0 para trazas ICMP
	# n = No trazas DNS

❯ tcpdump -i tun0 tcp                       # Solo traemos paquetes TCP
```

```bash
❯ tcpdump -D 
	# D = Muestre interfaces 
```

```bash
❯ tcpdump -c 100 -i tun0
	# c = Muestre un cierto numero de paquetes  
```

```bash
❯ tcpdump -A -i tun0
	# A = Nos trae paquetes en ASCII  
```

```bash
❯ tcpdump -w 0001.pcap -i tun0
	# w = Creamos un archivo .pcap llamado 0001, escuchara hasta que cortemos el trafico con 'Ctrl + c'

❯ tcpdump -r 0001.pcap
	# r = Leer el archivo .pcap
```




