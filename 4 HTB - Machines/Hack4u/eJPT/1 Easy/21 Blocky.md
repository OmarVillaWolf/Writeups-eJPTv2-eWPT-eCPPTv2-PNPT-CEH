## Summary

Tags: 
Y
- IP -> 10.10.10.37 
- Ports -> TCP (21,22,80,25565), UDP (idk)
- OS -> Linux Ubuntu Xenial 
- Services & Applications
    - 21 -> ftp PROFTPD 1.3.5a
    - 22 -> ssh OpenSSH 7.2p2 Ubuntu 4ubuntu2.2
    - 80 -> http Apache httpd 2.4.18
		Did not follow redirect to http://blocky.htb
    - 25565 -> minecraft 1.11.2

## Recon
- Comando -> **whatweb http://blocky.htb** Para analizar su gestor de contenido 
Miramos su pagina web para ver si podemos encontrar algo especifico como su panel de autenticacion:
	**wp-admin** Es la ruta del panel de autenticacion de wordpress

- Comando -> **wfuzz -c --hc=404 --hh=278 -t 200 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt http://blocky.htb/FUZZ** Hacemos fuzing a una pagina web especifica para poder encontrar subdominios con un diccionario especifico y encontramos (wp-content/, wiki, plugings/, wp-admin/) todos con codigo 301, los cuales los vamos probando en la pagina web

En el dominio de **plugings** encontramos dos archivos .jar los cuales al momento de darles click estos nos permiten descargar el archivo.

- Comando -> **install apt jd-gui** Nos abre archivos **.jar** en un GUI, para inspeccionar su codigo
- Comando -> **jg-gui** Para ejecutar el jd-gui en la consola y buscamos los archivos descargados de la pagina web


## User
- Comando -> **sshpass -p <'PASSWORD'> ssh < USER>@< IP>** Nos conectamos por ssh al usuario notch con la password que encontramos en el archivo que descargamos 

## Root
- Comando -> **sudo -l** Para ver los permisos en sudoers y nos damos cuenta que con la misma password nos podemos convertir en root
- Comando -> **sudo su** Nos convertimos en root colocando la misma password de notch