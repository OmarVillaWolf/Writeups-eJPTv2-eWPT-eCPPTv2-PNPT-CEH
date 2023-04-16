# Nmap ❮❯

Tags: #Nmap #Escaneo 

```bash
❯ nmap ❮Target_IP/24❯                      # Para escanear toda la red

	# Target IP = El rango de direccion a escanear 1.1.1.0/24 (Debe terminar en 0 con /24)
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts

	#  p = Escanea todos los puertos (65535)
	#  open = Muestra solo los puertos con un estatus “open”
	#  sS = Aplica un TCP SYN Scan
	#  min-rate 5000 = Indica que quiero emitir paquetes no más lentos que 5000 paquetes por segundo
	#  vvv = Muestra la información en pantalla a medida que se descubre
	#  n = Indica que no aplique resolución DNS
	#  Pn = Indica que no aplique el protocolo ARP
	#  Target IP = Dirección IP que se quiere escanear
	#  oG allPorts = Exporta el output a un fichero grepeable con nombre “allPorts”
```

```bash
❯ nmap -sCV -p22,80 ❮Target IP❯ -oN targeted

	#  p22,80 = Indica los puertos que se quieren escanear
	#  sC = Lanza scripts básicos de enumeración
	#  sV = Enumera la versión y servicio que está corriendo en los puertos
	#  Target IP = Dirección IP que se quiere escanear
	#  oN targeted = Exporta el output a un fichero en formato nmap con nombre “targeted”
```

```bash
❯ nmap --script "vuln and safe" -p445 ❮Target IP❯ -oN smbVulnScan   # Para ver si este servicio es vulnerable al ethernalblue (ms17-010) en Windows 7.

	#  p445 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  oN smbVulnScan = Exporta el output a un fichero en formato nmap con nombre “smbVulnScan”
	#  vuln and safe = Detecta vulnerabilidades de forma segura, sin experimentar un DoS
```

```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

	#  -p80 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  -oN WebScan = Exporta el output a un fichero en formato nmap con nombre “WebScan”
```

```bash
❯ nmap --script ssl-heartbleed -p8443 ❮Target IP❯ 	# heartbleed = Verifica si es vulnerable a Heartbleed

	#  -p8443 o 443 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
```

```bash
❯ nmap --script ftp-anon -p21 ❮Target IP❯              # ftp-enum = Escanea y mira si el usuario invitado 'Anonymous' esta habilitado

	#  p21 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
```