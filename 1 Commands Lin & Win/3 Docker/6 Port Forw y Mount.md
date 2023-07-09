# Port Forwarding en Docker y uso de monturas

Tags: #Docker #Comandos #Linux 

El **port forwarding**, también conocido como reenvío de puertos, nos permite **redirigir el tráfico de red** desde un puerto específico en el host a un puerto específico en el contenedor. Esto nos permitirá acceder a los servicios que se ejecutan dentro del contenedor desde el exterior.

```bash 
❯ docker run -dit -p 80:80 --name myWebServer webserver       # Para crear el contenedor le colocamos un nombre y colocamos la imagen antes creada y nos devuleve el identificador de nuestro contenedor

# Aplicaremos PortForwarding en donde indicamos que mi puerto 80 sea el puerto 80 del contenedor

❯ lsof -i:80                           # Para ver que servicio esta ocuoando cierto puerto
❯ docker port myWebServer              # Miramos el puerto que esta usando para ese servicio
❯ docker exec -it myWebSever bash      # Para conectarnos al contenedor existente y le ejecutamos el comando bash (Podemos colocarle cualquier comando)
	❯ cd /var/www/html                # La ruta tipica de Apache o Nginx en donde tenemos el index.html o el index.php
```

Docker Compose es una herramienta de orquestación de contenedores que permite definir y ejecutar aplicaciones multi-contenedor de manera fácil y eficiente. Con Docker Compose, podemos describir los diferentes servicios que componen nuestra aplicación en un **archivo YAML** y, a continuación, utilizar un solo comando para ejecutar y gestionar todos estos servicios de manera coordinada.

* Las **monturas**, por otro lado, nos permiten compartir un directorio o archivo entre el sistema host y el contenedor. Esto nos permitirá persistir la información entre ejecuciones de contenedores y compartir datos entre diferentes contenedores.

Para utilizar las monturas, se utiliza la opción “**-v**” o “**–volume**” en el comando “**docker run**“. Esta opción se utiliza para especificar la montura y se puede utilizar de varias maneras. Por ejemplo, si se desea montar el directorio “**/home/usuario/datos**” del host en el directorio “**/datos**” del contenedor

```bash 
❯ docker run -dit -p 80:80 -v /home/omar/Desktop/docker/:/var/www/html --name myWebServer webserver     # Para crear el contenedor le colocamos un nombre y colocamos la imagen antes creada y nos devuleve el identificador de nuestro contenedor

	# p = Aplicaremos PortForwarding en donde indicamos que mi puerto 80 sea el puerto 80 del contenedor
	# v = Para hacer que la primer ruta (Ruta principal) este montada en la segunda y con eso se lograra que cualquier cambio que haga en culaquier de las dos rutas se vea reflejado en ambas
```