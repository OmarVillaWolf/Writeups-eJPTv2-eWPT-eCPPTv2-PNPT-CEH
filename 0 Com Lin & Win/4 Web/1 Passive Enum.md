# Web Enumeration 

Tags: #Web #Enumeracion #WAF #Whois #DNSdumpster #Host #Netcraft #DNSrecon #HTTRack #EyeWitness #Sublist3r 

## Enumeración Web

```bash 
Enumeracion Pasiva 
1. Identificar nombres de dominios y titular 
2. Descubrir archivos o directorios escondidos o no permitidos 
3. Identificar la direccion IP del servidor web y su registro de DNS
4. Identificar tecnologias web que estan siendo usadas en el sitio 
5. Deteccion de WAF
6. Identificar subdominios 
7. Identificar la estructura del contenido del sitio web 

Enumeracion Activa 
1. Analizar y descargar el codigo fuente del sitio o aplicacion web 
2. Descubrimiento de puertos y servicios 
3. Fingerprinting al servidor web 
4. Escanear la aplicacion web 
5. Transferencia de zonas DNS
6. Enumeracion de los subdominios con Fuerza Bruta 
```

## Web Enumeration & Information Gathering

```bash 
❯ https://whois.domaintools.com/     # Nos regresa detalles y registros del dominio 
❯ whois <Domain name o IP>           # Comando por consola 
```

```bash 
❯ host <Domain name>                 # Nos muestra la IPv4 e IPv6 del dominio ya que es una utilidad DNS
```

```bash 
❯ https://www.netcraft.com/          # Usando la parte de "What's that site running" para detectar tecnologias en la web, informacion y ademas puede detectar si existe un WAF o un Proxy
```

```bash 
❯ https://dnsdumpster.com/           
❯ dnsrecon -d <Domain name>          # Checa todos los registros DNS 

	# A = Resuelve un dominio o hostname en una IPv4
	# AAAA = Resuelve un dominio o hostname en una IPv6
	# NS = Referencia al servidor de nombres de dominio
	# MX = Resuelve un dominio en un servidor de correo 
	# CNAME = Se usa para alias de dominio 
	# TXT = Registro de texto 
	# HINFO = Informacion del host 
	# SOA = Autoridad de dominio 
	# SRV = Registros de servicio 
	# PTR = Resuelve una direccion IP en un nombre de host 
```

```bash 
❯ https://github.com/EnableSecurity/wafw00f   # Tool de Github que detecta si existe un WAF en el dominio a analizar

❯ wafw00f -l                                  # Lista los firewalls que puede detectar 
❯ wafw00f <Domain name>                       # Nos muestra si ese dominio contiene un WAF
```

## Secure Code Analysis 

```bash 
❯ https://www.httrack.com/page/2/en/index.html      # Descargar la tool que nos ayudara a copiar un sitio web

❯ httrack www.domain.com -O Dir                     # Copiamos la web y agregamos el dir donde queremos que nos salve el index, los favicon, gif, etc... 
```

```bash 
❯ apt install eyewitness                      # Instalarlo en Kali 

❯ nvim domain.txt                             # Debemos de crear un archivo txt que contenga los dominios a usar 

❯ eyewitness --web -f domain.txt -d Dir       # Crea una screenshot de un sitio web 
	# f = Archivo que contiene los dominios 
	# d = Directorio en donde guardaremos la screenshot
```

## Subdomains

```bash 
❯ https://github.com/aboul3la/Sublist3r      # Tool para enumerar los subdominios

❯ sublist3r -d domain.com -e google.com      
	# d = Nombre del dominio a usar 
	# e = Buscador (Motor de busqueda) en alguno especifico, si no lo agregas lo hara en todos los motores de busqueda
``` 