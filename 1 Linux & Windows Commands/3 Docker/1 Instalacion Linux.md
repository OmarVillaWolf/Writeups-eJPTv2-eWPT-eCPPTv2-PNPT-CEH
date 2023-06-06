# Instalación de Docker en Linux

Tags: #Docker #Comandos #Linux 


**Docker** es una plataforma de **contenedores** de software que permite crear, distribuir y ejecutar aplicaciones en entornos aislados. Esto significa que se pueden empaquetar las aplicaciones con todas sus dependencias y configuraciones en un contenedor que se puede mover fácilmente de una máquina a otra, independientemente de la configuración del sistema operativo o del hardware.

Algunas de las ventajas que se presentan a la hora de practicar hacking usando Docker son:

-   **Aislamiento**: los contenedores de Docker están aislados entre sí, lo que significa que si una aplicación dentro de un contenedor es comprometida, el resto del sistema no se verá afectado.
-   **Portabilidad**: los contenedores de Docker se pueden mover fácilmente de un sistema a otro, lo que los hace ideales para desplegar entornos vulnerables para prácticas de hacking.
-   **Reproducibilidad**: los contenedores de Docker se pueden configurar de forma precisa y reproducible, lo que es importante en el hacking para poder recrear escenarios de ataque.

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