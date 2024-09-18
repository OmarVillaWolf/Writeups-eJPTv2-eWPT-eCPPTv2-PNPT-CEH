# Creación y construcción de imágenes

Tags: #Docker #Comandos #Linux 

Para crear una imagen de Docker, es necesario tener un archivo **Dockerfile** que defina la configuración de la imagen. Una vez que se tiene el Dockerfile, se puede utilizar el comando “**docker build**” para construir la imagen. Este comando buscará el archivo ‘Dockerfile’ en el directorio actual y utilizará las instrucciones definidas en el mismo para construir la imagen.

-   **docker build**: es el comando que se utiliza para construir una imagen de Docker a partir de un Dockerfile.

```bash 
❯ docker build -t my_first_image .     # Para construir nuestra primer imagen, con el punto buscara por el archivo Dockerfile en el directorio actual de trabajo (t=para ponerle un nombre y debe estar en minusculas para que no entre en conflicto)
❯ docker pull debian:latest            # Nos permite descargar una imagen de Docker de un registro de Docker

# Cabe mencionar que siempre debemos de ir modificando el archivo 'Dockerfile' y en cada cambio debemos de hacer un 'docker build'
```