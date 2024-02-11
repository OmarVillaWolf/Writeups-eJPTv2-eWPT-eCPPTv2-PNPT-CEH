# Maquina 1

## Summary

Tags: #eJPT #WordPress #MySQL #Hydra 

- IP ->  192.168.68.105
- Ports -> TCP (), UDP (idk)
- OS ->  
- Services & Applications
    -  -> 
    -  -> 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ nmap -sn ❮IP/24❯                  # Usara ARP para escanear la red y descubir los diferentes dispositivos en la red
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts 
❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

```bash 
❯ whatweb <IP>    
```

```bash 
# Podemos explorar la web e ir a las rutas mas comunes como:
	/phpinfo.php
	/wordpress/

# Cuando tenemos wn la web la pagina por default, debemos de hacer un 'Directory Listing'
❯ dirb http://IP /usr/share/dirb/wordlists/common.txt

# Encontraremos diferentes directorios como: 
	/admin/                                        # Ahi se encuentra el 'PhpMyAdmin'
	/Downloads/                                    # Directorio de descargas 
	/wordpress/ 
	/wordpress/wp-login.php                        # Panel de login de Wordpress 
	/wordpress/wp-admin/admin.php 
	/wordpress/wp-admin/user/
	/wordpress/wp-admin/plugins/
	/wordpress/wp-admin/plugins/index.php 

# Para analizar el WordPress podemos utilizar la tool 'wpscan' la cual nos data algunas vulnerabilidades 
❯ wpscan --url http://192.168.68.104/wordpress/ -e u    # Para enumerar usuarios de Wordpress
```

## User


## Root