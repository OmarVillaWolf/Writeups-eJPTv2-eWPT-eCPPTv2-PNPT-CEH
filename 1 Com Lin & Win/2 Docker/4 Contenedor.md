# Carga de instrucciones en Docker y desplegando nuestro primer contenedor

Tags: #Docker #Comandos #Linux 

El comando “**docker run \[options]**” se utiliza para crear y arrancar un contenedor a partir de una imagen. Algunas de las opciones más comunes para el comando “Docker run” son:

-   “**-d**” o “**–detach**“: se utiliza para arrancar el contenedor en segundo plano, en lugar de en primer plano.
-   “**-i**” o “**–interactive**“: se utiliza para permitir la entrada interactiva al contenedor.
-   “**-t**” o “**–tty**“: se utiliza para asignar un seudoterminal al contenedor (consola virtual). 
-   “**–name**“: se utiliza para asignar un nombre al contenedor y este ya puede llevar mayúsculas.

```bash 
❯ docker run -dit --name myContainer my_first_image      # Para crear el contenedor le colocamos un nombre y colocamos la imagen antes creada y nos devuleve el identificador de nuestro contenedor 
❯ docker exec -it myContainer bash                       # Para conectarnos al contenedor existente llamado myContainer y le ejecutamos el comando bash (Podemos colocarle cualquier comando)

# Cabe mencionar que al momento de crear el contenedor 'Propio' este te viene "desnudo" por lo que debemos de instalarle todo lo que vayamos a necesitar:
❯ apt update
❯ apt install net-tools -y
❯ apt install iputils-ping -y
❯ exit                           # Salir del Docker


# Pero si no queremos hacer esto manualmente podemos agregarlo en el archivo **Dockerfile** que anteriormente habiamos creado con el comando 'RUN'
❯ docker build -t my_first_image:v2 .                            # Para hacer otra imagen pero ahora con los nuevos cambios, con el punto buscara el archivo Dockerfile en el directorio actual de trabajo
❯ docker run -dit --name mySecondContainer my_first_image:v2     # Para crear el contenedor le colocamos un nombre y colocamos la imagen antes creada y nos devuleve el identificador de nuestro contenedor 
```