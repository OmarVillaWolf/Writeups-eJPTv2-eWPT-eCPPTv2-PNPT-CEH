## Uso de scripts y categorías en nmap para aplicar reconocimiento

Tags: #Nmap #Tool #Reconocimiento #Escaneo #Comandos 

❯ **nmap -p22 -sVC ❮IP❯** Para ver la Version y el Servicio que corren esos puertos (Fingerprinting)
* sV -> Detecta la version y el servicio que estan corriendo el puerto seleccionado
* sC -> Para lanzar un conjunto de scripts basicos de reconocimiento para que nos enumere mas informacion adicional

❯ **locate .nse** Podemos ver los scripts de Nmap que estan en hechos en Lua

❯ **locate ftp-anon.nse** Este script es para cuando estamos auditando un ftp, si esta habilitado el usuario anonymous nos lo reporta, y ahi podriamos conectarnos sin proporcionar passwd y hasta podriamos subir archivos.
❯ **locate http-robots.txt.nse** Este script es para validar la ruta **/robots.txt**, y si existe esta ruta podemos ver mas rutas que nos la reporta por consola 

Los scripts de nmap pueden tener una categoria o multiples categorias y son como 14
* 1.- Discovery - 2.- Default
* 3.- External - 4.- Version
* 5.- Safe - 6.- Auth
* 7.- Broadcast - 8.- DOS
* 9.- Brute - 10.- Exploit
* 11.- Intrusive - 12.- Fuzzer
* 13.- Vuln - 14.- Malware

-   **default**: Esta es la categoría predeterminada en Nmap, que incluye una gran cantidad de scripts de reconocimiento básicos y útiles para la mayoría de los escaneos.
-   **discovery**: Esta categoría se enfoca en descubrir información sobre la red, como la detección de hosts y dispositivos activos, y la resolución de nombres de dominio.
-   **safe**: Esta categoría incluye scripts que son considerados seguros y que no realizan actividades invasivas que puedan desencadenar una alerta de seguridad en la red.
-   **intrusive**: Esta categoría incluye scripts más invasivos que pueden ser detectados fácilmente por un sistema de detección de intrusos o un Firewall, pero que pueden proporcionar información valiosa sobre vulnerabilidades y debilidades en la red.
-   **vuln**: Esta categoría se enfoca específicamente en la detección de vulnerabilidades y debilidades en los sistemas y servicios que se están ejecutando en la red.

❯ **locate .nse | xargs grep "categories"** Para filtrar por las categorias de los scripts de Nmap (xargs=nos permite operar de forma paralela con otro comando)

❯ **nmap -p22 ❮IP❯ --script="vuln and safe"** Podemos mandar a escanear ciertas categorias, en este caso mandaremos puros scripts que usen las categorias 'vuln and safe' y en este caso estos actuan como Shakers 
❯ **nmap -p80 ❮IP❯ --script http-enum** Podemos hacer Fuzzing pero con un diccionario mas chico y hara fuerza bruta, este se usa cuando la web es muy simple y lo hara por el metodo GET y por el codigo de estado determinara si existe o no (200=Ok, 404=Not-Found, 500=Internal Server Error, 301,302=Redirect, 401=Unauthorized) pero solo nos reportara los que tengan un 200

## Creación de tus propios scripts en Lua para nmap
Los campos más comunes que se definen en la tabla de un script de Lua en Nmap incluyen:

-   **description**: Este campo se utiliza para proporcionar una descripción corta del script y su funcionalidad.
-   **categories**: Este campo se utiliza para especificar las categorías a las que pertenece el script, como descubrimiento, explotación, enumeración, etc.
-   **author**: Este campo se utiliza para identificar al autor del script.
-   **license**: Este campo se utiliza para especificar los términos de la licencia bajo la cual se distribuye el script.
-   **dependencies**: Este campo se utiliza para especificar cualquier dependencia de biblioteca o software que requiera el script para funcionar correctamente.
-   **actions**: Este campo se utiliza para definir la funcionalidad específica del script, como la realización de un escaneo de puertos, la detección de vulnerabilidades, etc.