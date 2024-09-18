# Definiendo la estructura básica de Dockerfile

Tags: #Docker #Comandos #Linux 

Un archivo **Dockerfile** se compone de varias secciones, cada una de las cuales comienza con una **palabra clave** en **mayúsculas**, seguida de uno o más argumentos.

Algunas de las secciones más comunes en un archivo Dockerfile son:

-   **FROM**: se utiliza para especificar la imagen base desde la cual se construirá la nueva imagen.
-   **RUN**: se utiliza para ejecutar comandos en el interior del contenedor, como la instalación de paquetes o la configuración del entorno.
-   **COPY**: se utiliza para copiar archivos desde el sistema host al interior del contenedor.
-   **CMD**: se utiliza para especificar el comando que se ejecutará cuando se arranque el contenedor.

```bash 
❯ nvim Dockerfile                                  # Nos creamos un archivo docker 
	❯ FROM ubuntu:latest                          # Para crear una imagen basada en Ubuntu
	❯ MAINTAINER Omar "omar\@outlook.com"         # Es opcional colocar datos de la persona
	❯ ENV DEBIAN_FRONTEND noninteractive          # Evitamos que entre en modo interactivo al momento de la instalacion 
	❯ RUN apt update && apt install -y net-tools \\
		iputils-ping \\
		curl \\
		git \\
		nano \\
		apache2 \\
		php\\** 
	❯ COPY prueba.txt /var/www/html                   # Le decimos que queremos traer archivos de mi equipo al contenedor y en que ruta lo colocamos
	❯ EXPOSE 80                                       # Exponer el puerto 80 el de la maquina como el mio
	❯ ENTRYPOINT service apache2 start && /bin/bash   # Para iniciar el servicio instalado y agregamos la bash para evitar que se apague el contenedor

# Colocamos la barra \\ para indicar que queremos colocar de una forma tabulada las herramientas que necesitaremos.
```