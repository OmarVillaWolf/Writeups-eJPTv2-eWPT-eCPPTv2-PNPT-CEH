# Wifi

Tags: #Linux #Wifi #Airmon-ng  #Aircrack #Airodump-ng #mdk4

## Recon 

```bash 
❯ iwconfig                          # Para ver el estado actual de la tarjetas wireless
❯ iwconfig <wlan>                   # Filtramos por una Wlan especifica
```

```bash 
# Configurar una interface en modo monitor 

❯ ip link set <wlan> down           
❯ iw <wlan> set monitor control 
❯ ip link set <wlan> up

o

❯ airmon-ng start <wlan>            # Iniciamos el modo monitor en la wlan (Le cambia el nombre a la interface)
❯ airmon-ng check kill              # Quitamos los procesos para despues poner la wlan en modo monitor 
❯ airmon-ng stop <wlan>             # Paramos el modo monitor
```

```bash 
# Quitar de modo monitor una interface wlan  

❯ ifconfig <wlan> down 
❯ iwconfig <wlan> mode managed
❯ service network-manager restart
```


## 01. List all client MACs
```bash 
❯ airodump-ng wlan0mon   # Para capturar paquetes, en el cual podemos ver (BSIID, Ciphers, CH, ENC, ESSID, Station(MAC), etc..)

# Atajos de airodump
	TAB                  # Seleccionar una fila especifica 
	a                    # Colocas los puntos de acceso y las 'station', o solo los puntos de acceso o solo las 'station'
	o                    # Activas el color 
	p                    # Desactivas el color 
	s                    # Ordenar entre clientes y AP (Access Point)
	barra space          # Pausa la captura y quita la pausa 

❯ airodump-ng wlan0mon --band abg --manufacturer --wps -w <file>      # Escanear todas las redes y el reultado lo guarda en varios archivos
	# manufactures = A veces nos da el nombre del fabricante
	# w = Que nos cree un archivo 'file.cap' y le asigna un identificador   
	# band abg = Escanear la frecuencia 2.4 GHz con 'bg' y 5 GHz con 'a'
	# wps = Muestra informacion WPS si hay 
	
❯ airodump-ng -c 44 --essid <name> wlan0mon --manufacturer --wps -w <file>   # Filtramos por el ESSID (Nombre)
	# c = Canal del essid
	# essid = Nombre del AP 
```

## 02. Detect APs information

```bash 
❯ airodump-ng wlan0mon --band abg --manufacturer --wps -w <file>      # Escanear todas las redes y el reultado lo guarda en varios archivos
```

## 03. Get probes from users

```bash 
❯ airodump-ng wlan0mon --band abg --manufacturer --wps -w <file>      # Escanear todas las redes y el reultado lo guarda en varios archivos
```

## 04. Find hidden network ESSID

```bash 
# Para encontrar un ESSID oculto debemos de hacer un ataque de fuerza bruta al 'Probes' para conseguir el nombre, esto lo podemos hacer con 'mdk4' 

❯ iwconfig wlan0mon channel 1         
	# channel = Canal en donde se encuentra el ESSID oculto 
❯ mdk4 wlan0mon p -t <BSSID> -f ~/rockyou.txt     # Ataque de fuerza bruta con el diccionario rockyou.txt
	# p = Ataque de desautenticación 
	# t = MAC del AP
	# f = Ruta absoluta del diccionario a usar
```