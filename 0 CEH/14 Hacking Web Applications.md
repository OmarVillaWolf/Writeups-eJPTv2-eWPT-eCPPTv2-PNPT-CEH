# Hacking Web Applications

Tags: #CEH #OWASP 

## OWASP TOP 10 - 2021

```bash 
* A01 - Broken Access Control   # Tener el acceso al contenido para usuarios privilegiados y restringidos. Es una falla en el control de acceso y puede pasar la autenticacion permitiendo comprometer la red ya que le permitiria actuar como un usuario admin con privilegio para 'crear, aaceder, actualizar o borrar' los registros. 

* A02 - Cryptographic Failures / Sensitive Data Exposure   # Algunas aplicaciones no protegen sus datos sensibles para usuarios no autorizados. La exposicion de datos se debe a la falla como almacenamiento criptografico inseguro e informacion filtrada. Cuando una app utiliza codigo de encirptacion baja, un atacante puede explotar esa falla y robar o modificar la data protegida sensible la cual podria ser 'numeros de tarjetas de credito, SSNs y otras credenciales de autenticacion'

* A03 - Injection Flaws   # Permite que la data no-verificada sea interpretada y ejecutada como parte de un comando o query. Inyectar estos comandos llevan a los atacantes a construir codigo malicioso o querys que resultan en perdida de datos, corrupcion de datos, DoS. Los fallos de inyeccion pueden ser encontrados en 'SQL, LDAP, XPath queries' y facilmente pueden ser descubiertos por escaners de aplicaciones y fuzzers. 
	* SQL Injection
	* Command injection 
	* LDAP injection 
	* XSS
	* Server-side JS injection
	* Server-side includes injection
	* Server-side template injection 
	* Log injection 
	* HTML injection 
	* CRLF injection 
	* JNDI injection 

* A04 - Insecure Design   # Es la inapropiada implementacion de controles de seguridad y conduce a vulnerabilidades cruciales como 'SQLi y Open S3 Buckets'. 

* A05 - Security Misconfiguration   # Son vulnerabilidades que no validan los inputs, parametros en los formularios. Un atacante puede ganar acceso a cuentas pos defaul, leer paginas sin uso, leer/escribir en archivos no protegidos y directorios. Esto puede ocurrie en cualquier nivel de la aplicacion incluyendo la plataforma, servicios web, servidor de aplicacion, framwork y codigo customizado. 
	* Validated inputs 
	* Parameter/Form tampering 
	* Improper error handling
	* Insufficient transport layer protection 
	* Improper restriction of XXE

* A06 -  Vulnerable and outdated components / Using components with known vulnerabilities   # Un atacante puede identificar componentes debiles o viejos con un analisis automatico o manual y pueden encontrar las vulnerabilidades en paginas como 'exploit-db.com'.

* A07 - Identification and Authentication Failures / Broken Authentication   # Un atacante puede explotar la vulnerabilidad en la identificacion, autenticacion o manejo de fucniones de sesion los cuales posen cuentas, sesiones ID, password, timeout, remember me, pregunta secreta, actualizacion de cuenta para impersonar a los usuarios. 

* A08 - Software and Data Integrity Failures   # Fallo en la actualizacion de aplicaciones con la ultima version o parche. Cuando instalan plugins, dependencias, librerias o paquetes de repocitorios publicos y la app confia en ellos. 

* A09 - Security Loggin and Monitoring Failures / Insufficient Logging and Monitoring   # Cuando un atacante hace muchos intentos de loggin hacia un sitio web y la aplicacion no nos muestre los 'logs' 

* A10 - Server-Side Request Forgery (SSRF)   # Es cuando a un servidor web se le envian peticiones especificas para poder llegar a un servidor interno de la red. 
	* Injecting an SSRF payload 
	* Cross-site port attack (XSPA)
```

## Methodology  tools
### Footprinting Web Structure

```bash 
# Tecnologias web 
❯ whatweb 
❯ wafw00f 

# DNS y direcciones IP del servidor 
❯ netcraft
❯ Smartwhois
❯ WHOIS Lookup
❯ Batch IP Converter 

# Locacion y tipos de servidores
❯ DNSRecon
❯ DNS Records
❯ Domain Dossier 
❯ DNSdumpster.com

# Servicios y puertos que existen en el servidor 
❯ Advanced Port Scanner
❯ Nmap / Zenmap 
❯ NetScanTools Pro
❯ Hping 

# Encontrar contendio oculto 
❯ OWASP Zed Attack Proxy

# Balanceadores de carga
❯ dig 
❯ lbd domain.com 
```

### Analizar aplicaciones web 

```bash 
# Tecnologias web 
❯ Wappalyzer

# Idenificar puntos de entrada 
❯ Burpsuite
❯ OWASP Zed Attack Proxy
❯ WebScarab
❯ httprint

# Identificar funcionalidad server-side
❯ GNU wget
❯ curl
❯ blackWindow

# Identificar archivos y directorios
❯ Gobuster 
❯ fuff 
❯ Dirbuster 

# Identificar vulnerabilidades en web apps 
❯ Vega
❯ WPScan
❯ Arachni
❯ appspider
❯ Uniscan
```

### Escaner de culnerabilidades REST API

```bash 
❯ Astra 
❯ Fuzzapi
❯ W3af
❯ appspider
❯ Vooki
❯ OWASP ZAP
```

### Web Shell Tools 

```bash 
❯ WSO Php Webshell
❯ Caterpillar Webshell
❯ b374k shell
❯ c99
❯ China Chopper
❯ R57
```

### Detectores de Web Shell

```bash 
❯ Web Shell Detector
❯ FireEye Network Security
❯ NeoPI
❯ AntiShell Web Shell Hunter
❯ Astra 
```