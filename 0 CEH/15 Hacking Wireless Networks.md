# Hacking Wireless Networks

Tags: #CEH 

## Tipos de Wireless 

```bash 
1. 802.11i   # Especifica los mecanismos para las redes wireless 802.11
2. WEP       # Algoritmo de encriptacion para IEEE 802.11 
3. EAP       # Soporta multiples metodos de autenticacion como 'token cards, kerberos' y certificados
4. LEAP      # Una version de Cisco de EAP
5. WPA       # Protocolo avanzado de encriptacion wireless usando TKIP y MIC para proveer autenticacion y encriptacion 
6. TKIP      # Un protocolo de seguridad usado en WPA como remplazo de WEP
7. WPA2      # Actualizacion de WPA usando AES y CCMP para la encipcion de data en wireless
8. AES       # Encripcion de llave simetrica, usada en WPA2 para una autenticacion y encriptacion fuerte 
9. CCMP      # Protocolo de encriptacion usado en WPA2 para una autenticacion y encriptacion fuerte 
10. WPA2 Enterprise   # Integra el estandar EAP con encriptacion WPA2
11. RADIUS   # Sistema de manejo centralizado para la autenticacion y autorizacion 
12. PEAP     # Protocolo que encapsula el EAP dentro de un encriptado y autenticado tunel TLS 'Transport Layer Security'
13. WPA3     # Protocolo de seguridad de tercera generacion de WiFi que usa AES-GCMP-256 para encriptacion y HMAC-SHA-384 para autenticacion  
```

## Ataques Wireless

```bash 
1. Ataques de control de acceso   # Evadir las medidas de control de acceso WLAN
	1. WarDriving 
	2. Rogue Acces Points        # Clonar un AP real y activarlo con el fin de tener conexion con clientes 
	3. MAC Spoofing              # Falsificar la MAC y hacer que una estacion se conecte al AP del atacante 
	4. AP Misconfiguration       # Colocar unn AP fuera del perimetro cooporativo para que los clientes se conecten 
	5. Ad Hoc Associations       # Proveer una red sin autenticacion en donde el atacante podria conectarse 
	6. Promiscuous Client 
	7. Client Mis-association     
	8. Unauthorized Association  # Crear un AP con el fin de infectar a la victima y asi poder ejecutar comandos, asi como tener una autenticacion no autorizada y poder hacer pivoting en la red 

2. Ataques de integridad          # Se hacen enviando control falsificado, manejo o dtat frames sobre una red wireless 
	1. Data Frame Injection 
	2. WEP Injection 
	3. Bit-Flipping Attacks 
	4. Extensible AP Replay 
	5. Data Replay 
	6. Initialization Vector Replay Attacks 
	7. RADIUS Replay 
	8. Wireless Network Viruses 

3. Ataques de confidencialidad    # Interceptan la informacion confidencial a traves de la asociacion wireless 
	1. Eavesdropping 
	2. Traffic Analysis
	3. Cracking WEP Key 
	4. Evil Twin AP
	5. Honeypot AP               # Crear un AP en un lugar publico para que los clientes se conecten 
	6. Session Hijacking 
	7. Masquerading 
	8. Man-in-the-middle Attack 


KRACK attack     # Se explota el '4-way handshake' del protocolo WPA2 forzando el 'Nonce reuse', esto pasa interceptando la llave de cifrado que viene desde el AP. Luego el atacante clona el AP y utiliza esa llave para mandarlo a la victima haciendo que se autentique al AP del atacante, por lo que ahora el atacante ve el trafico y luego le permite el paso al AP real 

Jamming attack   # Es un ataque DoS a una red wireless 2.4 GHz donde interrumpes la señal utilizando las mismas frecuencias  
	# CPB-3016N-5G Jammer
	# PCB-2040 Jammer
	# CPB-2060B Jammer 
	# CPB-2660H Jammer 
	# CPB-2061 Jammer 
	# CPB-2680-AGP Jammer  

aLTEr attack     # Ataque a dispositivos LTE 'celulares' creando una torre de comunicaciones falsa con el objetivo de que la victima se conecte a la torre falsa para despues permitirle el paso del trafico a la torre real 
```

## Wireless Hacking Tools 

```bash 
1. WiFi discovery 
❯ Wash utility          # Encontrar APs con 'WPS' activados como AP, ESSID y BSSID 
	❯ wash -i wlan0
❯ inSSIDer
❯ NetSurveyor

2. GPS Mapping 
❯ wiggle.net            # Mirar las redes inalambricas en cualquier parte del mundo 
❯ skyhook
❯ ExpertGPS
❯ GPS Visualizer
❯ Mapwel
❯ TrackMaker

3. Wireless Traffic Analysis 
❯ RF Explorer 

4. Launch of Wireless Attack 
❯ Aircrack-ng Suite     # Toda la suite para atacar redes WiFi
❯ MANA Toolkit          # Crear un Rogue attack 
❯ Flipper zero 

# Clonar RFID 
❯ RFIDler 
❯ Proxmark3
❯ RFID Mifare Cloner
❯ Boscloner Pro

5. Wi-Fi Encryption Cracking 
# Para WPA2 
❯ Aircrack-ng Suite     # Toda la suite para atacar redes WiFi
❯ Wifiphisher
❯ Reaver
❯ WPA/WPA2 Brute Forcing 

# Para WEP
❯ wesside-ng -i wlan0  

# Mas tools WEP/WPA/WPA2
❯ Wifite
❯ EAPHammer
❯ Portable Penetrator
❯ WepCrackGui
❯ Pyrit

# Para WPA3
❯ Podemos hacer 'downgrade' y forzar el WPA2
❯ Dragonblood           # Set de vulnerabilidades en WPA3
	❯ Dragonslayer
	❯ Dragonforce
	❯ Dragondrain
	❯ Dragontime
❯ Podemos hacer ataques de 'Side-channel' 
```

## Bluetooth Hacking Tools 

```bash 
1. Bluetooth reconnaissance 
❯ hciconfig                 # Escanear dispositivos Bluetooth 'pairable'
	❯ hciconfig scan
	❯ hciconfig inq        # Obtener mas info de los dispositivos descubiertos 

2. Bluetooth Attack 
❯ btlejack -d /dev/ttyACM0 -d /dev/ttyACM2 -s     # Seleccionar un dispositivo target 
❯ btlejack -s                                     # Sniffing una conexion existente 
❯ btlejack -c any                                 # Sniffing nuevas conexiones 
❯ btlejack -f 0x129f3244 -j                       # Hacer una operacion jamming 
❯ btlejack -f 0x9c68fd30 -t -m 0x1fffffffff       # Comnezar una conexion hijacking 

3. Bluetooth Encryption Cracking
❯ crackle -i <file.pcap>       
❯ crackle -i <file.pcap> -o <output.pcap>
❯ crackle -i <file.pcap> -l <LTK>
❯ crackle -i <file.pcap> -o <out.pcap> -l <LTK>

4. Hacking Tools 
❯ BluetoothView
❯ BTCrawler
❯ BlueScan
❯ Bluetooth Scanner - btCrawler
❯ Bluedevil
❯ Blueman
```