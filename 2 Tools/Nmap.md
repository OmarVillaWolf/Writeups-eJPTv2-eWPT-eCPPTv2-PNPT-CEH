# Nmap ❮❯

❯ **nmap ❮Target IP/24❯** Para escanear toda la red
* Target IP -> El rango de direccion a escanear 1.1.1.0/24 (Debe terminar en 0 con /24)

❯ **nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts**
-  -p- -> Escanea todos los puertos (65535)
-  –open -> Muestra solo los puertos con un estatus “open”
-  -sS -> Aplica un TCP SYN Scan
-  –min-rate 5000 -> Indica que quiero emitir paquetes no más lentos que 5000 paquetes por segundo
-  -vvv -> Muestra la información en pantalla a medida que se descubre
-  -n -> Indica que no aplique resolución DNS
-  -Pn -> Indica que no aplique el protocolo ARP
-  Target IP -> Dirección IP que se quiere escanear
-  -oG allPorts -> Exporta el output a un fichero grepeable con nombre “allPorts”

❯ **nmap -sCV -p22,80 ❮Target IP❯ -oN targeted**
-  -p22,80 -> Indica los puertos que se quieren escanear
-  -sC -> Lanza scripts básicos de enumeración
-  -sV -> Enumera la versión y servicio que está corriendo en los puertos
-  Target IP -> Dirección IP que se quiere escanear
-  -oN targeted -> Exporta el output a un fichero en formato nmap con nombre “targeted”

❯ **nmap --script "vuln and safe" -p445 ❮Target IP❯ -oN smbVulnScan**
-  -p445 -> Indica el puerto que se quiere escanear
-  Target IP -> Dirección IP que se quiere escanear
-  -oN smbVulnScan -> Exporta el output a un fichero en formato nmap con nombre “smbVulnScan”
-  vuln and safe -> Detecta vulnerabilidades de forma segura, sin experimentar un DoS
Para ver si este servicio es vulnerable al ethernalblue (ms17-010) en Windows 7.

❯ **nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan** 
-  -p80 -> Indica el puerto que se quiere escanear
-  Target IP -> Dirección IP que se quiere escanear
-  -oN WebScan -> Exporta el output a un fichero en formato nmap con nombre “WebScan”
-  http-enum -> Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

❯ **nmap --script ssl-heartbleed -p8443 ❮Target IP❯** 
-  -p8443 o 443 -> Indica el puerto que se quiere escanear
-  Target IP -> Dirección IP que se quiere escanear
- heartbleed -> Verifica si es vulnerable a Heartbleed

❯ **nmap --script ftp-anon -p21 ❮Target IP❯** 
-  -p21 -> Indica el puerto que se quiere escanear
-  Target IP -> Dirección IP que se quiere escanear
-  ftp-enum -> Escanea y mira si el usuario invitado 'Anonymous' esta habilitado


----
Los scrips de nmap siempre terminan **.nse** y estan programados en LUA.

65535 Puertos existen, 1024 (0-1023) Son los puertos concidos

80 HTTP
443 HTTPS
139 Windows NetBios
445 SMB

Los puertos en nmap, pueden ser Open, si es Closed la bandera es RST(Reset) o Filtered (Usualmente por un Firewall)

- Comando -> **nmap -h** Menu de ayuda de nmap
	**-sT** Conexion TCP
	**-sS** Conexion SYN "Half-Open" Scans
	**-sU** Conexion UDP
	**-O** Detectar el sistema operativo
	**-sV** Detecta la version de los servicios
	**-v** Incrementar el verbose (Tener mas informacion desplegada en el output de nmap). Este puede llegar a tener hasta 3 verbose para una mejor respuesta
	**-oA** Guardas el output del escaneo en nmap en los tres formatos Nmap
	**-oN** Guardas el output en formato Nmap
	**-oG** Guardas el output en formato Grepeable
	**-A** Escaneo agresivo
	**-p < Port>** Colocamos un puerto especifico
	**-p 1000-1500** Colocamos un rango especifico de puertos
	**-p-** Escaneamos los 65535 puertos (all ports)
	**--script** Para poder activar un script
	**--script=vuln** Vuln descubre las vulnerabilidades mas conocidas