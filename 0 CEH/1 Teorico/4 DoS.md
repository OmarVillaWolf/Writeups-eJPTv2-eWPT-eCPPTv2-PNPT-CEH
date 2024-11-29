# Denegación del servicio 

Tags: #CEH 

## Tools

```bash 
❯ High Orbit Ion Cannon (HOIC)
❯ Low Orbit Ion Cannon (LOIC)
❯ XOIC
❯ HULK
❯ Metasploit
❯ Tors Hammer 
❯ Slowloris
❯ PyLoris
```

## Vectores DoS/DDoS

```bash 
1. Ataques volumetricos    # Afectan el ancho de banda 'bits-per second (bps)', los cuales son: 
	1. UDP flood, ICMP flood
	2. Ping of death and smurf attack
	3. Pulse wave and zero-day attack 

2. Ataques de protocolo    # Afectan a los balanceadores de carga, firewalls, servidores de aplicaciones 'packets-per second (pps)', los cuales son: 
	1. SYN flood
	2. Fragmentation attack
	3. Spoofed session flood
	4. ACK flood
	5. TCP SACK panic attack

3. Ataques de capas de aplicacion   # Consume recursos o servicios de una aplicacion 'request-per second (rps)', los cuales son: 
	1. HTTP GET/POST attack
	2. Slowloris attack
	3. UDP application layer flood
	4. DDoS extortion attack 
```