# Enumeración local de Linux

Tags: #LocalEnumeration #PrivEsc #Linux #Meterpreter

## Enumeración de procesos y tareas Cron

```bash 
# Que estamos buscando?

1. Servicios que estan corriendo
2. Tareas Cron
```

```bash 
# Comandos con Meterpreter

❯ ps                     # Lista los procesos que estan corriendo, PID, User, Path
❯ pgrep <name>           # Filtras los procesos por nombre y te muestra el PID
```

```bash 
# Comandos Linux 

❯ ps                     # Lista los procesos que estan corriendo 
❯ ps aux                 # Despliega todos los procesos, User, ID, CPU, Path
❯ ps aux | grep <name>   # Filtras el proceso por proceso o usuario 
❯ top                    # Lista los procesos que estan siendo ejecutados 

❯ crontab -l             # Lista las tareas Cron del usuario al cual tienes acceso 
❯ ls -al /etc/cron*      # Despliega todos los archivos Cron por (dia, mes año)
❯ cat /etc/cron*          
```