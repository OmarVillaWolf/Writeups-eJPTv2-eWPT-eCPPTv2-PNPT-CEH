# Wifi

Tags: #Linux #Wifi #Airmon-ng #Aircrack #Airodump-ng 

## Comandos Wifi Linux

```bash 
❯ iwconfig                          # Para ver el estado actual de la tarjetas wireless
❯ iwconfig <wlan>                   # Filtramos por una Wlan especifica
```

```bash 
❯ ip link set <wlan> down           # Configurar una interface nueva en modo monitor 
❯ iw <wlan> set monitor control 
❯ ip link set <wlan> up

o

❯ airmon-ng check kill              # Quitamos los procesos para despues poner la wlan en modo monitor 
❯ airmon-ng start <wlan>            # Iniciamos el modo monitor en la wlan (Le cambia el nombre a la interface)

❯ airmon-ng stop <wlan>             # Paramos el modo monitor
 
```

```bash 
# Quitar de modo monitor una interface wlan  

❯ ifconfig <wlan> down 
❯ iwconfig <wlan> mode managed
❯ service network-manager restart
```

```bash 
❯ airodump-ng <wlan>   # Para capturar paquetes, en el cual podemos ver (BSIID, Ciphers, CH, ENC, ESSID, Station(MAC), etc..)

# Atajos de airodump
	TAB                  # Seleccionar una fila especifica 
	a                    # Colocas los puntos de acceso y las 'station', o solo los puntos de acceso o solo las 'station'
	o                    # Activas el color 
	p                    # Desactivas el color 
	s                    # Ordenar entre clientes y AP (Access Point)
	barra space          # Pausa la captura y quita la pausa 

❯ airodump-ng <wlan> --band abg --manufacturer -w <file>      # Escanear todas las redes y lo guarda en varios archivos
	# manufactures = A veces nos da el nombre del fabricante
	# w = Que nos cree un archivo 'file.cap' y le asigna un identificador   
	# band abg = Colocar la frecuencia que queremos que capture, en este caso 2.4 GHz con 'bg' y 5 GHz con 'a'

❯ airodump-ng --essid 'name' --band abg --manufacturer <wlan>    # Escaneo especifico de un ESSID en la banda 2.4 y 5 GHz 
```