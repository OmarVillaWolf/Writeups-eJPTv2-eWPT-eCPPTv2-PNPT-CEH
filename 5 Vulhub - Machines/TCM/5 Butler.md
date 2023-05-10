## Summary

Tags: #Windows #Jenkins 

- IP -> 192.168.68.2
- Ports -> TCP (7680, 8080), UDP (idk)
- OS ->  Windows
- Services & Applications
    - 8080 -> HTTP Jetty 9.4.41.v20210516


## Recon

```bash
❯ arp-scan -I ens33 --localnet --ignoredups              # Para hacer un escaneo de la red 

	# I = i mayuscula; es la interface ens33
	# ignoredups = Ignora IPs duplicadas 
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

Ahora miramos la web por le puerto 8080 y encontramos que es un **Jenkins** 



## User


## Root