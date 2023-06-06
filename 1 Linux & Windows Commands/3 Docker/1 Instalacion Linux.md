# Instalación de Docker en Linux

Tags: #Docker #Comandos #Linux 

```bash 
❯ apt install docker.io               # Para instalar Docker 
❯ service docker start                # Iniciamos el servicio de Docker
❯ service docker stop                 # Paramos el servicio de Docker

❯ docker images                       # Para mirar las imagenes existentes en Docker
❯ docker volume ls                    # Para mirar los volumenes existentes

❯ docker ps                           # Para ver los contenedores existentes que se estan ejecutando
	# “-a” o “–all“           =      se utiliza para listar todos los contenedores, incluyendo los contenedores detenidos.
	# “-q” o “–quiet“         =      se utiliza para mostrar sólo los identificadores numéricos de los contenedores.
```