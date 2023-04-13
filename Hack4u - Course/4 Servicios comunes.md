# Enumeracion de servicios comunes y gestores de contenido ❮❯

## Enumeración del servicio FTP

FTP es un protocolo ampliamente utilizado para la **transferencia de archivos** en redes. La enumeración del servicio FTP implica recopilar información relevante, como la versión del servidor FTP, la configuración de permisos de archivos, los usuarios y las contraseñas (mediante ataques de fuerza bruta o guessing), entre otros.

A continuación, se os proporciona el enlace al primer proyecto que tocamos en esta clase:

-   **Docker-FTP-Server**: [https://github.com/garethflowers/docker-ftp-server](https://github.com/garethflowers/docker-ftp-server)

Una de las herramientas que usamos en esta clase para el primer proyecto que nos descargamos es ‘**Hydra**‘. Hydra es una herramienta de pruebas de penetración de código abierto que se utiliza para realizar ataques de fuerza bruta contra sistemas y servicios protegidos por contraseña. La herramienta es altamente personalizable y admite una amplia gama de protocolos de red, como HTTP, FTP, SSH, Telnet, SMTP, entre otros.

El siguiente de los proyectos que utilizamos para desplegar el contenedor que permite la autenticación de usuarios invitados para FTP, es el proyecto ‘**docker-anon-ftp**‘ de ‘**metabrainz**‘. A continuación, se os proporciona el enlace al proyecto:

-   **Docker-ANON-FTP**: [https://github.com/metabrainz/docker-anon-ftp](https://github.com/metabrainz/docker-anon-ftp)


## Enumeración del servicio SSH

SSH es un protocolo de administración remota que permite a los usuarios **controlar** y **modificar** sus servidores remotos a través de Internet mediante un mecanismo de **autenticación seguro**. Como una alternativa más segura al protocolo **Telnet**, que transmite información sin cifrar, SSH utiliza **técnicas criptográficas** para garantizar que todas las comunicaciones hacia y desde el servidor remoto estén cifradas.

SSH proporciona un mecanismo para autenticar un usuario remoto, transferir entradas desde el cliente al host y retransmitir la salida de vuelta al cliente. Esto es especialmente útil para administrar sistemas remotos de manera segura y eficiente, sin tener que estar físicamente presentes en el sitio.

A continuación, se os proporciona el enlace directo a la web donde copiamos todo el comando de ‘docker’ para desplegar nuestro contenedor:
-   **Docker Hub OpenSSH-Server**: [https://hub.docker.com/r/linuxserver/openssh-server](https://hub.docker.com/r/linuxserver/openssh-server)

Cabe destacar que a través de la versión de SSH, también podemos identificar el codename de la distribución que se está ejecutando en el sistema.

Por ejemplo, si la versión del servidor SSH es “**OpenSSH 8.2p1 Ubuntu 4ubuntu0.5**“, podemos determinar que el sistema está ejecutando una distribución de Ubuntu. El número de versión “**4ubuntu0.5**” se refiere a la revisión específica del paquete de SSH en esa distribución de Ubuntu. A partir de esto, podemos identificar el **codename** de la distribución de Ubuntu, que en este caso sería “**Focal**” para Ubuntu 20.04.

Todas estas búsquedas las aplicamos sobre el siguiente dominio:
-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)


## Enumeración del servicio HTTP y HTTPS

**HTTP** (**Hypertext Transfer Protocol**) es un protocolo de comunicación utilizado para la transferencia de datos en la World Wide Web. Se utiliza para la transferencia de contenido de texto, imágenes, videos, hipervínculos, etc. El puerto predeterminado para HTTP es el puerto 80.

**HTTPS** (**Hypertext Transfer Protocol Secure**) es una **versión segura** de HTTP que utiliza SSL / TLS para cifrar la comunicación entre el cliente y el servidor. Utiliza el puerto 443 por defecto. La principal diferencia entre HTTP y HTTPS es que HTTPS utiliza una capa de seguridad adicional para cifrar los datos, lo que los hace más seguros para la transferencia.

Asimismo, otras de las herramientas que vemos en esta clase son ‘**sslyze**‘ y ‘**sslscan**‘. **Sslyze** es una herramienta de análisis de seguridad SSL que se utiliza para evaluar la configuración SSL de un servidor. Proporciona información detallada sobre el cifrado utilizado, los protocolos admitidos y los certificados SSL. **SSLScan** es otra herramienta de análisis de seguridad SSL que se utiliza para evaluar la configuración SSL de un servidor. Proporciona información detallada sobre los protocolos SSL / TLS admitidos, el cifrado utilizado y los certificados SSL.

La principal diferencia entre sslyze y sslscan es que sslyze se enfoca en la **evaluación** de la seguridad SSL/TLS de un servidor web mediante una exploración exhaustiva de los protocolos y configuraciones SSL/TLS, mientras que sslscan se enfoca en la **identificación** de los **protocolos** SSL/TLS admitidos por el servidor y los cifrados utilizados.

La identificación de las informaciones arrojadas por las herramientas de análisis SSL/TLS es de **suma importancia**, ya que nos puede permitir detectar vulnerabilidades en la configuración de un servidor y tomar medidas para proteger nuestra información confidencial.

**Heartbleed** es una vulnerabilidad de seguridad que afecta a la biblioteca OpenSSL y permite a los atacantes acceder a la memoria de un servidor vulnerable. Si un servidor web es vulnerable a Heartbleed y lo detectamos a través de estas herramientas, esto significa que un atacante podría potencialmente acceder a información confidencial, como claves privadas, nombres de usuario y contraseñas, etc.

A continuación, se proporciona el enlace al proyecto de Github donde desplegamos el laboratorio vulnerable a Heartbleed:
-   **CVE-2014-0160**: [https://github.com/vulhub/vulhub/tree/master/openssl/CVE-2014-0160](https://github.com/vulhub/vulhub/tree/master/openssl/CVE-2014-0160)


## Enumeración del servicio SMB

**SMB** significa **Server Message Block**, es un **protocolo** de comunicación de red utilizado para compartir archivos, impresoras y otros recursos entre dispositivos de red. Es un protocolo propietario de **Microsoft** que se utiliza en sistemas operativos **Windows**.

**Samba**, por otro lado, es una implementación libre y de código abierto del **protocolo SMB**, que se utiliza principalmente en sistemas operativos basados en **Unix** y **Linux**. Samba proporciona una manera de compartir archivos y recursos entre dispositivos de red que ejecutan sistemas operativos diferentes, como Windows y Linux.

Aunque SMB y Samba comparten una funcionalidad similar, existen algunas diferencias notables. SMB es un protocolo propietario de Microsoft, mientras que Samba es un proyecto de software libre y de código abierto. Además, SMB es una implementación más completa y compleja del protocolo, mientras que Samba es una implementación más ligera y limitada.

A continuación, se os comparte el enlace correspondiente al proyecto de Github que utilizamos para desplegar un laboratorio de práctica con el que poder enumerar y explotar el servicio Samba:

-   **Samba Authenticated RCE**: [https://github.com/vulhub/vulhub/tree/master/samba/CVE-2017-7494](https://github.com/vulhub/vulhub/tree/master/samba/CVE-2017-7494)

Una de las herramientas que utilizamos para la fase de reconocimiento es ‘**smbmap**‘. Smbmap es una herramienta de línea de comandos utilizada para enumerar recursos compartidos y permisos en un servidor SMB (Server Message Block) o Samba. Es una herramienta muy útil para la enumeración de redes y para la identificación de posibles vulnerabilidades de seguridad.

Con smbmap, puedes enumerar los recursos compartidos en un servidor SMB y obtener información detallada sobre cada recurso, como los permisos de acceso, los usuarios y grupos autorizados, y los archivos y carpetas compartidos. También puedes utilizar smbmap para identificar recursos compartidos que no requieren autenticación, lo que puede ser un problema de seguridad.

Además, smbmap permite a los administradores de sistemas y a los auditores de seguridad verificar rápidamente la configuración de permisos en los recursos compartidos en un servidor SMB, lo que puede ayudar a identificar posibles vulnerabilidades de seguridad y a tomar medidas para remediarlas.

A continuación, se proporciona una breve descripción de algunos de los parámetros comunes de smbmap:

-   **-H**: Este parámetro se utiliza para especificar la dirección IP o el nombre de host del servidor SMB al que se quiere conectarse.
-   **-P**: Este parámetro se utiliza para especificar el puerto TCP utilizado para la conexión SMB. El puerto predeterminado para SMB es el 445, pero si el servidor SMB está configurado para utilizar un puerto diferente, este parámetro debe ser utilizado para especificar el puerto correcto.
-   **-u**: Este parámetro se utiliza para especificar el nombre de usuario para la conexión SMB.
-   **-p**: Este parámetro se utiliza para especificar la contraseña para la conexión SMB.
-   **-d**: Este parámetro se utiliza para especificar el dominio al que pertenece el usuario que se está utilizando para la conexión SMB.
-   **-s**: Este parámetro se utiliza para especificar el recurso compartido específico que se quiere enumerar. Si no se especifica, smbmap intentará enumerar todos los recursos compartidos en el servidor SMB.

Asimismo, otra de las herramientas que se ven en esta clase es ‘**smbclient**‘. Smbclient es otra herramienta de línea de comandos utilizada para interactuar con servidores SMB y Samba, pero a diferencia de smbmap que se utiliza principalmente para enumeración, smbclient proporciona una interfaz de línea de comandos para interactuar con los recursos compartidos SMB y Samba, lo que permite la descarga y subida de archivos, la ejecución de comandos remotos, la navegación por el sistema de archivos remoto, entre otras funcionalidades.

En cuanto a los parámetros más comunes de smbclient, algunos de ellos son:

-   **-L**: Este parámetro se utiliza para enumerar los recursos compartidos disponibles en el servidor SMB o Samba.
-   **-U**: Este parámetro se utiliza para especificar el nombre de usuario y la contraseña utilizados para la autenticación con el servidor SMB o Samba.
-   **-c**: Este parámetro se utiliza para especificar un comando que se ejecutará en el servidor SMB o Samba.

Estos son algunos de los parámetros más comunes utilizados en smbclient, aunque hay otros disponibles. La lista completa de parámetros y sus descripciones se pueden encontrar en la documentación oficial de la herramienta.

Por último, otra de las herramientas que utilizamos al final de la clase para enumerar el servicio Samba es ‘**Crackmapexec**‘. CrackMapExec (también conocido como CME) es una herramienta de prueba de penetración de línea de comandos que se utiliza para realizar auditorías de seguridad en entornos de Active Directory. CME se basa en las bibliotecas de Python ‘**impacket**‘ y es compatible con sistemas operativos Windows, Linux y macOS.

**CME** puede utilizarse para realizar diversas tareas de auditoría en entornos de **Active Directory**, como enumerar usuarios y grupos, buscar contraseñas débiles, detectar sistemas vulnerables y buscar vectores de ataque. Además, CME también puede utilizarse para ejecutar ataques de diccionario de contraseñas, ataques de **Pass-the-Hash** y para explotar vulnerabilidades conocidas en sistemas Windows. Asimismo, cuenta con una amplia variedad de módulos y opciones de configuración, lo que la convierte en una herramienta muy flexible para la auditoría de seguridad de entornos de Active Directory. La herramienta permite automatizar muchas de las tareas de auditoría comunes, lo que ahorra tiempo y aumenta la eficiencia del proceso de auditoría.

A continuación, os compartimos el enlace directo a la Wiki para que podáis instalar la herramienta:

-   **CrackMapExec**: [https://wiki.porchetta.industries/getting-started/installation/installation-on-unix](https://wiki.porchetta.industries/getting-started/installation/installation-on-unix)







## Enumeración de gestores de contenido (CMS) – Joomla
Joomla es un sistema de gestión de contenidos (CMS) de código abierto que se utiliza para **crear sitios web** y **aplicaciones en línea**. Joomla es muy popular debido a su facilidad de uso y flexibilidad, lo que lo hace una opción popular para sitios web empresariales, gubernamentales y de organizaciones sin fines de lucro.

Joomla es altamente personalizable y cuenta con una gran cantidad de extensiones disponibles, lo que permite a los usuarios añadir funcionalidades adicionales a sus sitios web sin necesidad de conocimientos de programación avanzados. Joomla también cuenta con una comunidad activa de desarrolladores y usuarios que comparten sus conocimientos y recursos para mejorar el CMS.

A continuación, se comparte el enlace del proyecto que estaremos desplegando en Docker para auditar un Joomla:

-   **CVE-2015-8562**: [https://github.com/vulhub/vulhub/tree/master/joomla/CVE-2015-8562](https://github.com/vulhub/vulhub/tree/master/joomla/CVE-2015-8562)

Una de las herramientas que usamos en esta clase es **Joomscan**. Joomscan es una herramienta de línea de comandos diseñada específicamente para escanear sitios web que utilizan Joomla y buscar posibles vulnerabilidades y debilidades de seguridad.

Joomscan utiliza una variedad de técnicas de enumeración para identificar información sobre el sitio web de Joomla, como la versión de Joomla utilizada, los plugins y módulos instalados y los usuarios registrados en el sitio. También utiliza una base de datos de vulnerabilidades conocidas para buscar posibles vulnerabilidades en la instalación de Joomla.

Para utilizar Joomscan, primero debemos descargar la herramienta desde su sitio web oficial. A continuación se os proporciona el enlace al proyecto:

-   **Joomscan**: [https://github.com/OWASP/joomscan](https://github.com/OWASP/joomscan)

Una vez descargado, podemos utilizar la siguiente sintaxis básica para escanear un sitio web de Joomla:

➜ `perl joomscan.pl -u ❮URL❯`

Donde ❮URL❯ es la URL del sitio web que deseamos escanear. Joomscan escaneará el sitio web y nos proporcionará una lista detallada de posibles vulnerabilidades y debilidades de seguridad.

Es importante tener en cuenta que joomscan **no es una herramienta infalible** y puede generar **falsos positivos** o **falsos negativos**. Por lo tanto, es importante utilizar joomscan junto con otras herramientas y técnicas de seguridad para tener una imagen completa de la seguridad del sitio web de Joomla que estemos auditando.

## Enumeración de gestores de contenido (CMS) – Drupal


Drupal ofrece un alto grado de personalización y escalabilidad, lo que lo convierte en una opción popular para sitios web complejos y grandes. Drupal se utiliza en una amplia gama de sitios web, desde blogs personales hasta sitios web gubernamentales y empresariales. Es altamente flexible y cuenta con una amplia variedad de módulos y herramientas que permiten a los usuarios personalizar su sitio web para satisfacer sus necesidades específicas.

Una de las herramientas que veremos en esta clase para enumerar un Drupal es la herramienta **droopescan**. Droopescan es una herramienta de escaneo de seguridad especializada en la identificación de versiones de Drupal y sus módulos, y en la detección de vulnerabilidades conocidas en ellos. La herramienta realiza un escaneo exhaustivo del sitio web para encontrar versiones de Drupal instaladas, módulos activos y vulnerabilidades conocidas, lo que ayuda a los administradores de sistemas y desarrolladores a identificar y solucionar los problemas de seguridad en sus sitios web.

Con esta herramienta, se pueden llevar a cabo análisis de seguridad en sitios web basados en Drupal, lo que puede ayudar a prevenir posibles ataques y problemas de seguridad en el futuro.

A continuación, es proporciona el enlace directo al proyecto en Github:

-   **Droopescan**: [https://github.com/SamJoan/droopescan](https://github.com/SamJoan/droopescan)

Su uso es bastante intuitivo, a continuación se comparte un ejemplo de uso de esta herramienta:

➜ `droopescan scan drupal --url https://example.com`

Donde “**scan**” indica que queremos realizar un escaneo, “**drupal**” especifica que estamos realizando un escaneo de Drupal y “**–url https://example.com**” indica la URL del sitio web que se va a escanear.

Asimismo, os compartimos a continuación el enlace al proyecto de Github correspondiente al laboratorio que estaremos desplegando en Docker:

-   **CVE-2018-7600**: [https://github.com/vulhub/vulhub/tree/master/drupal/CVE-2018-7600](https://github.com/vulhub/vulhub/tree/master/drupal/CVE-2018-7600)


## Enumeración de gestores de contenido (CMS) – Magento

Magento es una plataforma de comercio electrónico de código abierto, que se utiliza para construir tiendas en línea de alta calidad y escalables. Es una de las plataformas más populares para el comercio electrónico y es utilizado por grandes marcas como Nike, Coca-Cola y Ford.

Sin embargo, con la popularidad de Magento también ha surgido la preocupación por la seguridad. Una de las herramientas que veremos en esta clase es **Magescan**, una herramienta de escaneo de vulnerabilidades específica para Magento.

Magescan puede detectar vulnerabilidades comunes en Magento, incluyendo problemas con permisos de archivos, errores de configuración y vulnerabilidades conocidas en extensiones populares de Magento.

A continuación se proporciona el enlace directo al proyecto en Github:

-   **Magescan**: [https://github.com/steverobbins/magescan](https://github.com/steverobbins/magescan)

Su sintaxis y modo de uso es bastante sencillo, a continuación se comparte un ejemplo:

➜ `php magescan.phar scan:all https://example.com`

Donde “**magescan.pha**r” es el archivo ejecutable de la herramienta “**Magescan**“, “**scan:all**” es el comando específico de Magescan que indica que se realizará un escaneo exhaustivo de todas las vulnerabilidades conocidas en el sitio web objetivo y “**https://example.com**” es la URL del sitio web objetivo que se escaneará en busca de vulnerabilidades.

Asimismo, se comparte el enlace al laboratorio que estaremos desplegando en Docker para configurar el Magento vulnerable:

-   **Magento 2.2 SQL Injection**: [https://github.com/vulhub/vulhub/tree/master/magento/2.2-sqli](https://github.com/vulhub/vulhub/tree/master/magento/2.2-sqli)

Un ataque de inyección SQL exitoso puede permitir al atacante obtener información confidencial, como credenciales de usuario o datos de pago, o incluso ejecutar comandos en la base de datos del sitio web.

En el caso del Magento que estaremos desplegando, explotaremos una inyección SQL con el objetivo de obtener una cookie de sesión, la cual podremos posteriormente utilizar para llevar a cabo un ataque de “**Cookie Hijacking**“. Este tipo de ataque nos permitirá como atacantes asumir la identidad del usuario legítimo y acceder a las funciones del usuario, que en este caso será administrador.