## Summary

Tags: #Linux 

- IP -> 10.10.10.242
- Ports -> TCP (22,80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> OpenSSH 8.2p1
    - 80 ->  Apache httpd 2.4.41

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ ping -c 1 ❮IP❯                             # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)
```

```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

	#  -p80 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  -oN WebScan = Exporta el output a un fichero en formato nmap con nombre “WebScan”
```
Encontramos que hay un folder llamado 'icons' 

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```
Tenemos un Apache 2.4.41, un PHP 8.1.0-dev

```bash
❯ wfuzz -c -L --hc=404 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.10.242/FUZZ
```


## User


## Root