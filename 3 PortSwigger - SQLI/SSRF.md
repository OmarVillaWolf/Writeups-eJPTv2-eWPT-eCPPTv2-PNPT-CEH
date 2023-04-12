# Server Side Request Forgery 'PortSwigger' 

Esta vulnerabilidad ocurre cuando una aplicación web permite hacer consultas HTTP del lado del servidor hacia un dominio arbitrario elegido por el atacante. Esto le permite a un atacante hacer conexión con servicios de la infraestructura interna donde se aloja la web y exfiltrar información sensible.

Un atacante podría realizar consultas a servicios internos de la empresa para:

-   Robar datos sensibles como credenciales de usuarios o archivos de sistema.
-   Hacer peticiones a servicios internos para manejar el panel de administración, escanear puertos y servicios dentro de la infraestructura interna y conectarse al servidor de correos para enviar correos sin autorización.
-   Escalar privilegios dentro del sistema y ejecutar código de forma remota dentro del servidor.

Para todos los lab, debemos des desplegar la imagen en la pagina de PortSwigger, para poder hacer el SSRF.
**Ctrl + r** Mandamos la informacion al Repeater en BurpSuite 
**Ctrl + u** Hacemos url encode a la informacion 
**Ctrl + Shift + u** Quitamos el url encode a la informacion
**Ctrl + I** Mandamos lo del Repeater al Intruder (I-i latina) que nos ayudara a poder hacer Brute Forse


La maquina internamente puede tener un puerto abierto 8080 que externamente no se ve. Por lo que podemos ver por el puerto 80 consultas internas que hace el mismo puerto 80 al servidor y ver lo que hay en el puerto 8080 por su mismo localhost. 
Basicamente la misma maquina puede ver lo que hay en ella internamente por un puerto expuesto.

Todos los lab se hacen metiendonos a 'View Details' y con **BurpSuite** podemos capturar la parte de 'Check Stock'

### Lab 1 Basic SSRF against the local server 

![[Pasted image 20230224161949.png]]


Debemos de ingresar a http://localhost/admin

Nos vamos a enfocar en la parte de stockappi:http%3A%2F... 
Por lo que lo url-decodeamos para poder empezar a hacer el SSRF

stockappi:http://stock.weliketheshop.net:8080/product/...
Ademas debemos de url-encoder el **&** -> **%26** ya que la mayor parte de las veces no funciona bien en la web cuando lo mandas por BurpSuite

Una vez que vemos que funciona, debemos de quitar la url anterior y colocar la que nos proporciona el lab de esta manera:
stockappi:http://localhost/admin Y miramos que nos carga el panel de admin, por lo que podemos borrar a Carlos que es el objetivo de la maquina.

Ahora ponemos en formato RAW la parte derecha del BurpSuite en donde se ve el panel de admin y filtramos por Carlos. Podriamos ver la instruccion y asi agregarla a la url anterior para poder borrar al usuario.

stockappi:http://localhost/admin/delete?username=carlos


### Lab 2 Basic SSRF against another back-end system
Podemos enumerar otra servicios expuestos de otrso servidores dentro de la red interna. 

El objetico de este la es ver la ip 192.168.0.x en donde hay una interface por el puerto 8080 y borrar a Carlos.

En este tipo de casos debemos de usar el Intruder de BurpSuite y ahi el campo que iremos cambiando es el ultimo numero de la ip que esta en el campo:
stockappi:http://192.168.0.1:8080/admin
Seleccionamos el campo o en este caso el numero 1 que es el que ira cambiando, y le damos en **add** para que nos lo cambie a un payload

Despues en el Intruder nos vamos a la parte de **Payloads** 
	payload set: 1
	payload type: numbers

Number range
	From: 1
	To: 254
	Step: 1

Y finalizamos dandole a **start attack**

Despues miraremos el status 200 que indica que ese numero si esta dentro del rango de las direcciones que existen en esa red.
Encontramos la 219
stockappi:http://192.168.0.219:8080/admin En esa direccion encontramos el panel del admin

Ahora solo queda agregar lo ultimo para poder borrar a Carlos
stockappi:http://192.168.0.219:8080/admin/delete?username=carlos

Al final le damos en Follow Redirection 

### Lab 3 SSRF with blacklist-based input filter
