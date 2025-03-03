# Ataques a redes Wifi 

Tags: #Hacking #Wifi #Ñ 

## WEP

```bash 
# Kali Tool
❯ aircrack-ng 'WEPcrack-01.cap'      # Si se obtiene un archivo con el 'Handshake' se puede obtener la passwd 
```

## WPA2

```bash 
# Kali Tool
❯ aircrack-ng -a2 -b Target_BSSID -w /home/kali/Desktop/Wordlist/password.txt 'WPA2crack-01.cap'

	# a = Es la técnica '2=WPA'
	# b = BSSID es la MAC Address del access point
	# w = Diccionario de contraseñas 
```