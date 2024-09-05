## Summary

Tags: #Linux #HTTP #Capabilities #Nodejs #Nginx #HTML #ReverShell #Gobuster #Dirbuster #SSTI #BurpSuite #SubDomains #Wfuzz 

- IP -> 10.10.11.122
- Ports -> TCP (22, 80, 443), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> 
    - 80 -> Nginx 1.18.0 Ubuntu 
    - 443 -> Nginx 1.18.0 Ubuntu 

## Launchpad

-   [Launchpad](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ whatweb ❮http://❮IP❯      # Miramos las tecnologias 

# Agregamos el dominio que encontramos al '/etc/hosts'

❯ openssl s_client -connect ❮dominio.com❯:443   # Miramos los certificados en busqueda de otro dominio 
```

```bash
# Enumerar subdominios 

❯ gobuster vhost --append-domain -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --url https://nunchucks.com -t 200 -k 

	# append-domain = Enumerar los subdominios
	# vhost = Modo enumeracion VHost Subdominios
	# w = Ruta del diccionario
	# t = Lanzar tareas en paralelo al mismo tiempo
	# k = Para certificados autofirmados para el puerto 443


❯ wfuzz -c --hc=404 --hh=12345 -t 200 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H “Host: FUZZ.nunchucks.htb” https://nunchucks.htb

	# hc = HideCode 404
	# c = Formato colorido
	# hh = HideCharacters 12345
	# w = Ruta del diccionario
	# FUZZ = Donde va a insertar las palabras el diccionario
	# t = Lanzar tareas en paralelo al mismo tiempo
	# H = Para enumerar el subdominio, utilizamos esta cabecera 'Host: '
```



## User


## Root