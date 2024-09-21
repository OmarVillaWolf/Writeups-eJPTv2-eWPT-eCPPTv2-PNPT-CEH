# Evading IDS, Firewalls and Honeypots

Tags: #CEH 


```bash 
1. IDS 'Intrusion Detection System'  # Es un sistema de software o dispositivo de hardware que inspecciona todas las entradas y salidas de trafico, cuando detecta una anomalia envia alarmas. 
	1. Basados en red
	2. Basados en host 
# Tipos de alertas 
	1. True positive 
	2. False positive
	3. False negative
	4. True negative

2. IPS 'Intrusion Prevention System'  # Es considerado un IDS activo, no solo detecta intrusiones sino que tambien las previene el cual se situa detras de los firewall, analiza el trafico de red y toma decisiones automaticas del trafico que entra a la red  

3. Firewall   # Son diseñados para prevenir acceso no autorizado a la red privada. Son colocados entre el 'gateway' y las dos redes privada como publica. Examina todos los mensajes de entrada y salida de Intranet (Red privada) para permitir o no el trafico. Existen dos tipos: 
	1. Hardware
	2. Software 
# Tecnologias para crear un Firewall
	1. Packet filtering 
	2. Circuit level gateways 
	3. Aplication level firewall 'WAF'
	4. Stateful multilayer inspection 
	5. Application proxies 
	6. Network address translation 
	7. Virtual private network 

4. Honeypot   # Dispositivo que aparenta tener vulnerabilidades, abriendo muchos puertos, mostrando un sistema debil con el objetivo de ponerle una trampa a un atacante con el fin de obtener alertas tempranas, ademas, captura todo lo que el atacabte esta tecleando y algunos de ellos son:
	1. Low-interaction        # Limitan el numero de servicios 
	2. Medium-interaction     # Es un sistema real de operacion 
	3. High-interaction       # Simula todos los servicios 
	4. Pure honeypots         # Red de produccion real 
```

## Tools

```bash 
1. Deteccion de malware
❯ YarGen 
	❯ yaraRET
	❯ Koodous 
	❯ AutoYara
	❯ Halogen
	❯ Yabin

2. IDS
❯ Snort 
❯ Suricata
❯ AlienVault OSSIM (SIEM)
❯ SolarWinds Security Event Manager 
❯ OSSEC
❯ BroIDS/Zeek IDS

3. IPS
❯ USM Anywhere 
❯ IBM Security Network Intrision Prevention System
❯ Cyberoam Intrision Prevention System
❯ McAfee Host Intrusion Prevention for Desktops 
❯ Secure IPS (NGIPS)
❯ Quantum Intrusion Prevention System (IPS)

4. Firewalls 
❯ ZoneAlarm Free Firewall 
❯ ManageEngine Firewall Analyzer 
❯ pfSense
❯ Sophos XG Firewall
❯ Comodo Firewall 
❯ Palo Alto Network Wildfire 

5. Honeypots
❯ KFSensor
❯ HneyBOT

```

## Evasión Firewall   

```bash 
1. Evadir Firewall con proxy con el 'Google Translator' con los siguientes pasos:
	1. Ir a la pagina 'https://www.ipchicken.com/'
	2. Colocar la url anterior en 'Google translator' y traducirla 
	3. Abrir el enlace que nos da 'Google Translator'

2. Evadir Firewall con ICMP Tunneling 
3. Evadir Firewall con ACK Tunneling 
❯ AckCmd

4. Evadir Firewalls con HTTP Tunneling
❯ HTTPort and HTTHost 
❯ Super Network Tunnel 

5. Evadir Firewalls con SSH
❯ ssh -f user@domain.com -L 5000:domain.com:25 -N
	# f = Modo background 
	# L = Local port, host remote 
	# N = No ejecuta el comando en el servidor remoto 
❯ Bitvise 
❯ Secure pipes 

6. Evadir Firewalls con DNS Tunneling 
7. Evadir Firewalls con sistemas externos 
8. Evadir Firewalls con Windows BITS
```

## Evasión WAF

```bash 
1. Evadir WAF con XSS
2. Using HTTP headers Spoofing 
3. Using blacklist detection 

4. Using Fuzzing / Brute-forcing
❯ Seclist Fuzzing Database 

5. Abusing SSL / TLS ciphers 
❯ sslscan 

6. HTML Smuggling 
```

## Evasión IDS tools

```bash 
❯ Traffic IQ Professional 
❯ Namp 
❯ Metasploit
❯ Inundator 
❯ IDS-Evasion
❯ Hyperion-2.3.1
```

## Generadores de paquetes fragmentados

```bash 
❯ Colasoft Packet Builder 
❯ CommView
❯ NetScan Tools Pro
❯ Ostinato
❯ WAN Killer 
❯ WireEdit 
```

## Detectar Honeypots tools

```bash 
❯ Send-safe Honeypot Hunter 
❯ kippo_detect
```