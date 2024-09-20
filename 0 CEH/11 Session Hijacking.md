# Session Hijacking 

Tags: #CEH 

## Tools

```bash 
❯ Hetty 
❯ Bettercap 
❯ OWASP ZAP
❯ Burpsuite 
❯ Netool toolkit 
❯ Websploit framework 
❯ sslstrip
❯ JHijack

# Detection tools
❯ Wireshark 
❯ USM anywhere
```

```bash 
# Tipos
1. Pasiva    # Un atacante roba la sesion pero solo visualiza y graba el trafico de la sesion 
2. Activa    # Un atacante encuentra una sesion activa y toma el control de esa sesion 


# Modelo OSI
1. Capa de red            # Intercepcion de paquetes durante la transmision entre el cliente y servidor por TCP o UDP y puede ser comprometido de las siguientes maneras:
	1. Blind hijacking 
	2. UDP hijacking 
	3. TCP/IP hijacking 
	4. RST hijacking 
	5. Man-in-the-middle: Packet sniffer 
	6. IP spoofing: Source routed packets

2. Capa de aplicacion     # Ganar el control de de la sesion HTTP del usuario obteniendo el ID de la sesion y puede ser comprometido de las siguientes maneras:
	1. Session sniffing 
	2. Man-in-the-middle attack 
	3. XSS
	4. Session replay attack 
	5. CRIME attack 
	6. Session donation attack 
	7. Predictable session 
	8. Man-in-the-browser attack 
	9. CSRF
	10. Session fixation 
	11. Forbidden 
	12. PetitPotam hijacking 
```