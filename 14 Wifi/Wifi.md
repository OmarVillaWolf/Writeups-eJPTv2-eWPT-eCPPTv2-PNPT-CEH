# Wifi

Tags: #Linux #Wifi 

## Comandos Wifi Linux

```bash 
❯ iwconfig                  # Miramos las interfaces Wifi y podemos identificar la que se encuentra en modo 'monitor'
❯ iwconfig <wlan>           # Filtramos por una Wlan especifica
```

```bash 
❯ iwconfig <wlan> mode manage      # Si sale error debemos de hacer lo siguiente
	❯ airmon-ng stop <wlan>       # Paramos el modo monitor (Le coloca un nuevo nombre a la Wlan)
	❯ airmon-ng check kill 
	❯ airmon-ng start <wlan>      # Iniciamos el modo monitor para capturar paquetes 
```

```bash 
❯ airodump-ng <wlanmon>   # Para capturar paquetes, en el cual podemos ver (BSIID, Ciphers, CH, ENC, ESSID, Station(MAC), etc..)

# Atajos de airodump
	TAB                  # Seleccionar una fila especifica 
	a                    # Colocas los puntos de acceso y las 'station', o solo los puntos de acceso o solo las 'station'
	o                    # Activas el color 
	p                    # Desactivas el color 
	barra space          # Pausa y despausa

❯ airodump-ng <wlanmon> --band abg --manufacturer -w <file> # Guardas el escaneo en un archivo 
	# manufactures = A veces nos da el nombre del fabricante
	# w = Que nos cree un archivo 'file.cap' y le asigna un identificador   
	# band abg = Colocar la frecuencia que queremos que capture, en este caso 2.4 GHz con 'bg' y 5 GHz con 'a'
```