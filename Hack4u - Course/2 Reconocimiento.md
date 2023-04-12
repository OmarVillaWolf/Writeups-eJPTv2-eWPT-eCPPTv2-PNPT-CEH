# Reconocimiento ❮❯

## Nmap y sus diferentes modos de escaneo

### Escaneo por TCP
❯ **nmap -sn ❮IP/24❯** Aplica un barrido con ping, para ver por trazas ICMP ver si el equipo esta activo
* IP -> Direccion para hacer el barrido y debe terminar con **.0/24**
❯ **nmap -sn ❮IP/24❯ | grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}' | sort** Aplica un barrido con ping y solo mostrara las IP que encuentre y las ordenara

❯ **nmap -p- --open -T5 -sT ❮IP❯ -v -n -Pn** Para que nos devuelva los diferentes puertos que encuentre (Open, Filtered)
* p -> Escanea todo el rango de puertos 65535, puedes especificar un puerto a escanear asi: **-p22** etc...
* IP -> Direccion a escanear
* v -> Verbose y lo que hace es que nos muestra mas detalles al momento de ejecutar el programa
* open -> Escanea solo los puertos abiertos 
* n -> Quitamos la resolucion DNS
* T -> Nos permite controlar la plantilla de temporisado (0,1,2,,4,5)
* sT -> Manda un **SYN -> (RST (Cerrado) | SYN/ACK (Abierto)) -> ACK (Established))**
* Pn -> Indica que todas las direcciones son marcadas como UP

❯ **nmap -p22,80 -sVC ❮IP❯** Para ver la Version y el Servicio que corren esos puertos (Fingerprinting)
* sV -> Detecta la version y el servicio que estan corriendo el puerto seleccionado
* sC -> Para lanzar un conjunto de scripts basicos de reconocimiento para que nos enumere mas informacion adicional

❯ **nmap --top-ports 500 ❮IP❯** Para que nos escanee los 500 puertos mas comunes, si no se especifica, lo hace por TCP
* top-ports -> Para escanear los 500 puertos mas comunes  

❯ **nmap -O ❮IP❯** Para detectar el OS (**Pero no es recomendable**)
* O -> Detecta cual es el sistema operativo


### Escaneo por UDP
❯ **nmap -p- --open  -sU ❮IP❯ -v -n -Pn** Para que nos devuelva los diferentes puertos que encuentre (Open, Filtered)
* p -> Escanea todo el rango de puertos 65535, puedes especificar un puerto a escanear asi: **-p22** etc...
* IP -> Direccion a escanear
* v -> Verbose y lo que hace es que nos muestra mas detalles al momento de ejecutar el programa
* open -> Escanea solo los puertos abiertos 
* n -> Quitamos la resolucion DNS
* Pn -> Indica que todas las direcciones son marcadas como UP
* sU -> Escanear por el protocolo UDP


❯ **nmap --top-ports 500 --open -sU ❮IP❯ -v -n -Pn** Para que nos escanee los 500 puertos mas comunes, si no se especifica, lo hace por TCP
* top-ports -> Para escanear los 500 puertos mas comunes  por UDP


## Técnicas de evasión de Firewalls / IDS y Spoofing (MTU, Data Length, Source Port, Decoy, etc.)
**Firewall:** Es un sistema de seguridad de red que supervisa y controla el trafico de red entrante y saliente en funciones de reglas de seguridad predeterminadas.
**IDS**: Es un Intrution Detection System que sirve para evitra accesos no autorizados 

❯ **nmap -p22 ❮IP❯ -f** Para mandar paquetes fragmentados y asi evadir el Firewall
❯ **nmap -p22 ❮IP❯ --mtu 16** Si mandamos el MTU inferior al que se espera, podriamos burlar el Firewall (Este valor debe ser multiplo de 8)
❯ **nmap -p22 ❮IP❯ -D ❮IP❯,❮IP❯,❮IP❯** Podemos escribir las direcciones IP que querramos y asi hacer Spoofing al puerto seleccionado, esto funciona en que a veces el Firewall solo permite cierta IP para ver cierto puerto abierto, entonces asi podriamos llegar a ver un puerto mas con otra direccion IP, tambien hacemos que el Firewall no pueda detetctar la IP que esta haciendo el escaneo
❯ **nmap -p22 ❮IP❯ --open -T5 -n -v --source-port 53** Podemos modificar el puerto aleatorio de salida que abre nuestra maquina al momento de lanzar el escaneo y enviar la solicitud, esto ayuda en que dependiendo del puerto de origen, muchas veces nos muestra mas info y 'evadiriamos el Firewall'
❯ **nmap -p22 ❮IP❯ --data-length 21** Podemos modificar la longitud del paquete, esto nos ayudara a evadir el Firewall, ya que muchas veces los Firewalls tienen cierta longitud que no admiten, esto lo haremos sumandole ciertas cantidades a la longitud como por ejemplo +21
❯ **nmap -p22 ❮IP❯ -Pn --spoof-mac Dell** Podemos falsificar direcciones MAC y colocarle la MAC de otro vendor, ya que muchas veces ciertas MAC no son admitidas por el Firewall
❯ **nmap -p-  --open -sS --min-rate 5000 -v -n -Pn ❮IP❯** Lo que hacemos con sS -> es que al momento de ver que el puerto esta abierto no respondemos con ACK, si no que respondemos con RST y esto hace que cerremos la conexion directamente, y no dejamos logs, por lo que el Firewall no lo identificara, ya que muchos identifican conexiones completas.

Algunos de los parámetros vistos en esta clase son los siguientes:

* **MTU (–mtu):** La técnica de evasión de MTU o “Maximum Transmission Unit” implica ajustar el tamaño de los paquetes que se envían para evitar la detección por parte del Firewall. Nmap permite configurar manualmente el tamaño máximo de los paquetes para garantizar que sean lo suficientemente pequeños para pasar por el Firewall sin ser detectados.
* **Data Length (–data-length):** Esta técnica se basa en ajustar la longitud de los datos enviados para que sean lo suficientemente cortos como para pasar por el Firewall sin ser detectados. Nmap permite a los usuarios configurar manualmente la longitud de los datos enviados para que sean lo suficientemente pequeños para evadir la detección del Firewall.
* **Source Port (–source-port):** Esta técnica consiste en configurar manualmente el número de puerto de origen de los paquetes enviados para evitar la detección por parte del Firewall. Nmap permite a los usuarios especificar manualmente un puerto de origen aleatorio o un puerto específico para evadir la detección del Firewall.
* **Decoy (-D):** Esta técnica de evasión en Nmap permite al usuario enviar paquetes falsos a la red para confundir a los sistemas de detección de intrusos y evitar la detección del Firewall. El comando -D permite al usuario enviar paquetes falsos junto con los paquetes reales de escaneo para ocultar su actividad.
* **Fragmented (-f):** Esta técnica se basa en fragmentar los paquetes enviados para que el Firewall no pueda reconocer el tráfico como un escaneo. La opción -f en Nmap permite fragmentar los paquetes y enviarlos por separado para evitar la detección del Firewall.
* **Spoof-Mac (–spoof-mac):** Esta técnica de evasión se basa en cambiar la dirección MAC del paquete para evitar la detección del Firewall. Nmap permite al usuario configurar manualmente la dirección MAC para evitar ser detectado por el Firewall.
* **Stealth Scan (-sS):** Esta técnica es una de las más utilizadas para realizar escaneos sigilosos y evitar la detección del Firewall. El comando -sS permite a los usuarios realizar un escaneo de tipo SYN sin establecer una conexión completa, lo que permite evitar la detección del Firewall.
* **min-rate (–min-rate):** Esta técnica permite al usuario controlar la velocidad de los paquetes enviados para evitar la detección del Firewall. El comando –min-rate permite al usuario reducir la velocidad de los paquetes enviados para evitar ser detectado por el Firewall.


## Uso de scripts y categorías en nmap para aplicar reconocimiento

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

## Descubrimiento de equipos en la red local (ARP e ICMP) y Tips

❯ **nmap -sn ❮IP/24❯** Aplica un barrido con ping, para ver por trazas ICMP ver si el equipo esta activo
* IP -> Direccion para hacer el barrido y debe terminar con **.0/24**

❯ **arp-scan -I ens33 --localnet** Para hacer un escaneo de la red (I=i mayuscula interface) en la interface ens33
❯ **arp-scan -I ens33 --localnet --ignoredups** Para hacer un escaneo de la red (I=i mayuscula interface) en la interface ens33 y que no salgan duplicados 

**Tip: Si nos dan una IP 192.168.11.2/24 con esa mascara que seria de clase C, podriamos hacer el escaneo con una mascara de clase B -> asi 192.168.0.0/16 en lugar de 192.168.11.0/24**

❯ **masscan -p21,22,139,445,80,8080,443 -Pn ❮IP/16❯ --rate=5000** Es una herramienta como nmap que sirve para aplicar escaneos a nivel de red, pero mas potente y rapida
* Escanea millones de host por minuto
* En un simple paquete te descubre todos los puertos abiertos que pueda tener un unico host

## Validación del objetivo (Fijando un target en HackerOne)

https://www.hackerone.com/ En esta pagina de HackerOne es para encontrar vulnerabilidades a las companias 
* En la seccion de **Hacktivity** podemos observar las vulnerabilidades que han encontrado, reportado y en donde 

## Descubrimiento de correos electrónicos
A continuación, se proporcionan los enlaces a las herramientas online vistas en esta clase:

-   Hunter: [https://hunter.io/](https://hunter.io/)
-   Intelligence X: [https://intelx.io/](https://intelx.io/)
-   Phonebook.cz: [https://phonebook.cz/](https://phonebook.cz/)
-   Clearbit Connect: [Chrome Extension](https://chrome.google.com/webstore/detail/clearbit-connect-free-ver/pmnhcgfcafcnkbengdcanjablaabjplo)

Paginas para verificar los correos electronicos 
* Email-Checker: [Email-Checker](https://email-checker.net)
* Verify-Email-Address: [VerifyEmailAddress.org](https://www.verifyemailaddress.org)

## Reconocimiento de imágenes

El funcionamiento de PimEyes se basa en el análisis de **patrones faciales**, que son comparados con una base de datos de imágenes en línea para encontrar similitudes. La plataforma también permite buscar imágenes de personas que aparecen en una foto en particular, lo que puede ser útil en la investigación de casos de acoso o en la búsqueda de personas desaparecidas.
Esta herramienta recolecta informacion de la imagen (cara), puedes hacer 3 reconcimientos por dia gratis y si pagas hasta 25 por dia

-   PimEyes: [https://pimeyes.com/en](https://pimeyes.com/en)

Una de las herramientas en línea que vemos en esta clase es ‘**PimEyes**‘. PimEyes es una plataforma en línea que utiliza tecnología de **reconocimiento facial** para buscar imágenes similares en Internet en función de una imagen que se le proporciona como entrada. Esta herramienta puede ser útil en la detección de información personal de una persona, como sus perfiles en redes sociales, direcciones de correo electrónico, números de teléfono, nombres y apellidos, etc.


## Enumeración de subdominios
A continuación, os adjuntamos los enlaces a las herramientas vistas en esta clase:

-   **Phonebook** (Herramienta pasiva): [https://phonebook.cz/](https://phonebook.cz/)
-   **Intelx** (Herramienta pasiva): [https://intelx.io/](https://intelx.io/)
-   **CTFR** (Herramienta pasiva): [https://github.com/UnaPibaGeek/ctfr](https://github.com/UnaPibaGeek/ctfr)
-   **Gobuster** (Herramienta activa): [https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)
-   **Wfuzz** (Herramienta activa): [https://github.com/xmendez/wfuzz](https://github.com/xmendez/wfuzz)
-   **Sublist3r** (Herramienta pasiva): [https://github.com/aboul3la/Sublist3r](https://github.com/aboul3la/Sublist3r)


La enumeración de **subdominios** es una de las fases cruciales en la seguridad informática para identificar los subdominios asociados a un dominio principal.
Los subdominios son parte de un dominio más grande y a menudo están configurados para apuntar a diferentes recursos de la red, como servidores web, servidores de correo electrónico, sistemas de bases de datos, sistemas de gestión de contenido, entre otros.
Al identificar los subdominios vinculados a un dominio principal, un atacante podría obtener información valiosa para cada uno de estos, lo que le podría llevar a encontrar **vectores de ataque potenciales**. Por ejemplo, si se identifica un subdominio que apunta a un servidor web vulnerable, el atacante podría utilizar esta información para intentar explotar la vulnerabilidad y acceder al servidor en cuestión.


## Credenciales y brechas de seguridad
La seguridad de la información es un tema crítico en el mundo digital actual, especialmente cuando se trata de datos sensibles como **contraseñas**, **información financiera** o de **identidad**. Los ataques informáticos son una amenaza constante para cualquier empresa u organización, y una de las principales técnicas utilizadas por los atacantes es la **explotación de las credenciales** y **brechas de seguridad**.

-   **DeHashed**: [https://www.dehashed.com/](https://www.dehashed.com/)


## Identificación de las tecnologías en una página web

La herramienta **whatweb** es una utilidad de análisis de vulnerabilidades que escanea la página web y proporciona información detallada sobre las tecnologías utilizadas. Esta herramienta también puede utilizarse para identificar posibles vulnerabilidades y puntos débiles en la página web.

**Wappalyzer**, por otro lado, es una extensión del navegador que detecta y muestra las tecnologías utilizadas en la página web. Esta herramienta es especialmente útil para los expertos en seguridad que desean identificar rápidamente las tecnologías utilizadas en una página web sin tener que realizar un escaneo completo.

**Builtwith.com** es una herramienta en línea que también permite identificar las tecnologías utilizadas en una página web. Esta herramienta proporciona información detallada sobre las tecnologías utilizadas, así como también estadísticas útiles como el tráfico y la popularidad de la página web.

A continuación, os proporcionamos los enlaces correspondientes a las herramientas vistas en esta clase:

-   **Whatweb**: [https://github.com/urbanadventurer/WhatWeb](https://github.com/urbanadventurer/WhatWeb)
-   **Wappalyzer**: [https://addons.mozilla.org/es/firefox/addon/wappalyzer/](https://addons.mozilla.org/es/firefox/addon/wappalyzer/)
-   **Builtwith**: [https://builtwith.com/](https://builtwith.com/)


## Fuzzing y enumeración de archivos en un servidor web

**Wfuzz** es una herramienta de descubrimiento de contenido y una herramienta de inyección de datos. Básicamente, se utiliza para automatizar los procesos de prueba de vulnerabilidades en aplicaciones web.

Permite realizar ataques de fuerza bruta en parámetros y directorios de una aplicación web para identificar recursos existentes. Una de las **ventajas** de Wfuzz es que es altamente personalizable y se puede ajustar a diferentes necesidades de pruebas. Algunas de las **desventajas** de Wfuzz incluyen la necesidad de comprender la sintaxis de sus comandos y que puede ser más lenta en comparación con otras herramientas de descubrimiento de contenido.

Por otro lado, **Gobuster** es una herramienta de descubrimiento de contenido que también se utiliza para buscar archivos y directorios ocultos en una aplicación web. Al igual que Wfuzz, Gobuster se basa en ataques de fuerza bruta para encontrar archivos y directorios ocultos. Una de las principales **ventajas** de Gobuster es su velocidad, ya que es conocida por ser una de las herramientas de descubrimiento de contenido más rápidas. También es fácil de usar y su sintaxis es simple. Sin embargo, una **desventaja** de Gobuster es que puede no ser tan personalizable como Wfuzz.

A continuación, te proporcionamos el enlace a estas herramientas:

-   **Wfuzz**: [https://github.com/xmendez/wfuzz](https://github.com/xmendez/wfuzz)
-   **Gobuster**: [https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)



## BurpSuite 
### BurpSuite Community Edition

Es la **versión gratuita** de esta plataforma, viene incluida por defecto en el sistema operativo. Su función principal es desempeñar el papel de **proxy HTTP** para la aplicación, facilitando la realización de pruebas de penetración.

Un **proxy HTTP** es un filtro de contenido de alto rendimiento, ampliamente usado en el hacking con el fin de interceptar el tráfico de red. Esto permite analizar, modificar, aceptar o rechazar todas las solicitudes y respuestas de la aplicación que se esté auditando.

Algunas de las ventajas que la versión gratuita ofrecen son:

-   **Gratuidad**: La versión Community Edition es gratuita, lo que la convierte en una opción accesible para principiantes y profesionales con presupuestos limitados.
-   **Herramientas básicas**: Incluye las herramientas esenciales para realizar pruebas de penetración en aplicaciones web, como el Proxy, el Repeater y el Sequencer.
-   **Intercepción y modificación de tráfico**: Permite interceptar y modificar las solicitudes y respuestas HTTP/HTTPS, facilitando la identificación de vulnerabilidades y la exploración de posibles ataques.
-   **Facilidad de uso**: La interfaz de usuario de la Community Edition es intuitiva y fácil de utilizar, lo que facilita su adopción por parte de usuarios con diversos niveles de experiencia.
-   **Aprendizaje y familiarización**: La versión gratuita permite a los usuarios aprender y familiarizarse con las funcionalidades y técnicas de pruebas de penetración antes de dar el salto a la versión Professional.
-   **Comunidad de usuarios**: La versión Community Edition cuenta con una amplia comunidad de usuarios que comparten sus conocimientos y experiencias en foros y blogs, lo que puede ser de gran ayuda para resolver problemas y aprender nuevas técnicas.

### BurpSuite Proffesional

BurpSuite Proffessional es la **versión de pago** desarrollada por la empresa **PortSwigger**. Incluye, además del proxy HTTP, algunas herramientas de pentesting web como:

-   **Escáner de seguridad automatizado**: Permite identificar vulnerabilidades en aplicaciones web de manera rápida y eficiente, lo que ahorra tiempo y esfuerzo.
-   **Integración con otras herramientas**: Puede integrarse con otras soluciones de seguridad y entornos de desarrollo para mejorar la eficacia de las pruebas.
-   **Extensibilidad**: A través de su API, BurpSuite Professional permite a los usuarios crear y añadir extensiones personalizadas para adaptarse a necesidades específicas.
-   **Actualizaciones frecuentes**: La versión profesional recibe actualizaciones periódicas que incluyen nuevas funcionalidades y mejoras de rendimiento.
-   **Soporte técnico**: Los usuarios de BurpSuite Professional tienen acceso a un soporte técnico de calidad para resolver dudas y problemas.
-   **Informes personalizables**: La herramienta permite generar informes detallados y personalizados sobre las pruebas de penetración y los resultados obtenidos.
-   **Interfaz de usuario intuitiva**: La interfaz de BurpSuite Professional es fácil de utilizar y permite a los profesionales de seguridad trabajar de manera eficiente.
-   **Herramientas avanzadas**: Incluye funcionalidades avanzadas, como el módulo de intrusión, el rastreador de vulnerabilidades y el generador de payloads, que facilitan la identificación y explotación de vulnerabilidades en aplicaciones web.


## Google Dorks / Google Hacking (Los 18 Dorks más usados)
El ‘**Google Dork**‘ es una técnica de búsqueda avanzada que utiliza operadores y palabras clave específicas en el buscador de Google para encontrar información que normalmente no aparece en los resultados de búsqueda regulares.

La técnica de ‘**Google Dorking**‘ se utiliza a menudo en el hacking para encontrar información sensible y crítica en línea. Es una forma eficaz de recopilar información valiosa de una organización o individuo que puede ser utilizada para realizar pruebas de penetración y otros fines de seguridad.

Al utilizar Google Dorks, un atacante puede buscar información como nombres de usuarios y contraseñas, archivos confidenciales, información de bases de datos, números de tarjetas de crédito y otra información crítica. También pueden utilizar esta técnica para identificar vulnerabilidades en aplicaciones web, sitios web y otros sistemas en línea.

Es importante tener en cuenta que la técnica de Google Dorking **no es ilegal en sí misma**, pero puede ser utilizada con fines maliciosos. Por lo tanto, es crucial utilizar esta técnica con responsabilidad y ética en el contexto de la seguridad informática y el hacking ético.


Estos son ejemplos de algunos
* **site:** tinder.com -> Que solo me muestre sitios web
* **site:** \*.tinder.com -> Que solo me muestre sitios web pero que inicien con algo y despues tengan el dominio especificado
* **inurl:** wp-config.php.txt -> Decir que dentro de la url exista ese archivo 
* **filetype:** txt -> Que solo busque ese tipo de archivo (pdf,txt,docx,excel, etc...)
* **intext:** tinder.com -> De lo que encuentres en la entrada debe de contener ese dominio

Esta pagina recoge muchos Google Dorks y  ahi buscaremos Google Hacking, para despues colocar el dominio y escoger un Google Dorks 
* Pentest Tools: [Pentest Tools](https://pentest-tools.com/)

En esta pagina podemos ir a GHDB y ahi podemos ver un almacen pero especifico de Dorks
- ExploitDB: [ExploitDB](https://www.exploit-db.com/) 


## Identificación y verificación externa de la versión del sistema operativo
El tiempo de vida (**TTL**) hace referencia a la cantidad de tiempo o “**saltos**” que se ha establecido que un paquete debe existir dentro de una red antes de ser descartado por un enrutador. El TTL también se utiliza en otros contextos, como el almacenamiento en caché de CDN y el almacenamiento en caché de DNS.

Cuando se crea un paquete de información y se envía a través de Internet, está el riesgo de que siga pasando de enrutador a enrutador indefinidamente. Para mitigar esta posibilidad, los paquetes se diseñan con una caducidad denominada **tiempo de vida** o **límite de saltos**. El TTL de los paquetes también puede ser útil para determinar cuánto tiempo ha estado en circulación un paquete determinado, y permite que el remitente pueda recibir información sobre la trayectoria de un paquete a través de Internet.

Cada paquete tiene un lugar en el que se almacena un valor numérico que determina cuánto tiempo debe seguir moviéndose por la red. Cada vez que un enrutador recibe un paquete, resta uno al recuento de TTL y lo pasa al siguiente lugar de la red. Si en algún momento el recuento de TTL llega a cero después de la resta, el enrutador descartará el paquete y enviará un mensaje ICMP al host de origen.

Si enviamos un paquete a una máquina y recibimos una respuesta que tiene un valor TTL de **128**, es probable que la máquina esté ejecutando **Windows**. Si recibimos una respuesta con un valor TTL de **64**, es más probable que la máquina esté ejecutando **Linux**.

A continuación, se os comparte la página que mostramos en esta clase para identificar el sistema operativo correspondiente a los diferentes valores de TTL existentes.
-   **Subin’s Blog**: [https://subinsb.com/default-device-ttl-values/](https://subinsb.com/default-device-ttl-values/)

Asimismo, os compartimos el script de Python encargado de identificar el sistema operativo en función del TTL obtenido:
-   **WhichSystem**: [https://pastebin.com/HmBcu7j2](https://pastebin.com/HmBcu7j2)


