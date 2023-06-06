## Enumeración del servicio HTTP y HTTPS

Tags: #HTTP #HTTPS #Web #Enumeracion 

**HTTP** (**Hypertext Transfer Protocol**) es un protocolo de comunicación utilizado para la transferencia de datos en la World Wide Web. Se utiliza para la transferencia de contenido de texto, imágenes, videos, hipervínculos, etc. El puerto predeterminado para HTTP es el puerto 80.

**HTTPS** (**Hypertext Transfer Protocol Secure**) es una **versión segura** de HTTP que utiliza SSL / TLS para cifrar la comunicación entre el cliente y el servidor. Utiliza el puerto 443 por defecto. La principal diferencia entre HTTP y HTTPS es que HTTPS utiliza una capa de seguridad adicional para cifrar los datos, lo que los hace más seguros para la transferencia.

Asimismo, otras de las herramientas que vemos en esta clase son ‘**sslyze**‘ y ‘**sslscan**‘. **Sslyze** es una herramienta de análisis de seguridad SSL que se utiliza para evaluar la configuración SSL de un servidor. Proporciona información detallada sobre el cifrado utilizado, los protocolos admitidos y los certificados SSL. **SSLScan** es otra herramienta de análisis de seguridad SSL que se utiliza para evaluar la configuración SSL de un servidor. Proporciona información detallada sobre los protocolos SSL / TLS admitidos, el cifrado utilizado y los certificados SSL.

La principal diferencia entre sslyze y sslscan es que sslyze se enfoca en la **evaluación** de la seguridad SSL/TLS de un servidor web mediante una exploración exhaustiva de los protocolos y configuraciones SSL/TLS, mientras que sslscan se enfoca en la **identificación** de los **protocolos** SSL/TLS admitidos por el servidor y los cifrados utilizados.

La identificación de las informaciones arrojadas por las herramientas de análisis SSL/TLS es de **suma importancia**, ya que nos puede permitir detectar vulnerabilidades en la configuración de un servidor y tomar medidas para proteger nuestra información confidencial.

**Heartbleed** es una vulnerabilidad de seguridad que afecta a la biblioteca OpenSSL y permite a los atacantes acceder a la memoria de un servidor vulnerable. Si un servidor web es vulnerable a Heartbleed y lo detectamos a través de estas herramientas, esto significa que un atacante podría potencialmente acceder a información confidencial, como claves privadas, nombres de usuario y contraseñas, etc.

A continuación, se proporciona el enlace al proyecto de Github donde desplegamos el laboratorio vulnerable a Heartbleed:
-   **CVE-2014-0160**: [https://github.com/vulhub/vulhub/tree/master/openssl/CVE-2014-0160](https://github.com/vulhub/vulhub/tree/master/openssl/CVE-2014-0160)