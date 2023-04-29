# Escalacion de privilegios Linux

Tags: #Linux #Escalada #Root #Privilegios #Capabilities #SUID #LSE #PSPY #LinPEAS #CRON #Netstat #GCC

* HackTricks: [Hacktricks](https://book.hacktricks.xyz/welcome/readme)
* GTFOBins: [GTFOBins](https://gtfobins.github.io/)

## Enumeracion Tools Automaticas Enumeration

-   **Herramienta LSE**: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)

Esta herramienta nos ayudar a identificar vulnerabilidades para poderla explotar y escalar privilegios.
	**lse.sh** > Mirar en modo Raw
	Copiar la url resultante en Raw 

```bash
❯ wget https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh # Binario que debemos de descargar
❯ chmod +x lse.                     # Le damos permisos de ejecucion 
❯ ./lse.sh                          # Ejecutamos el binario

	# h = Panel de ayuda
``` 


-   **Herramienta PSPY**: [https://github.com/DominicBreuker/pspy](https://github.com/DominicBreuker/pspy)

Esta herramienta nos ayudara a identificar tareas, procesos que se estan ejecutando.
	Releases > Improved troubleshooting > **pspy64** -> Binario que debemos de descargar

```bash
❯ ./pspy                            # Ejecutamos el binario 
```


* **Herramienta LinPEAS**: 



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

* [Dirty-Pipe]()

```bash
❯ which gcc                                       #

	# /usr/bin/gcc = Podemos usar el Dirty-Pipe el cual nos ayudara a sobre-escribir el /etc/passwd 
```

#### Privilegios SUID

* **GTFOBins:** [GTFOBins](https://gtfobins.github.io/)
Podemos ver en esa pagina que comando podemos usar si encontramos un permiso SUID
```bash 
❯ find / -perm -4000 -ls 2>/dev/null               # Buscaremos por privilegios SUID, con ls = Miramos los privilegios y buscamos los de root

	# /usr/bin/pkexec 
	# 
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
