## Enumeración de gestores de contenido (CMS) – Magento

Tags: #CMS #Web 

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