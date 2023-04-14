## Identificación y verificación externa de la versión del sistema operativo

Tags: #Enumeracion 

El tiempo de vida (**TTL**) hace referencia a la cantidad de tiempo o “**saltos**” que se ha establecido que un paquete debe existir dentro de una red antes de ser descartado por un enrutador. El TTL también se utiliza en otros contextos, como el almacenamiento en caché de CDN y el almacenamiento en caché de DNS.

Cuando se crea un paquete de información y se envía a través de Internet, está el riesgo de que siga pasando de enrutador a enrutador indefinidamente. Para mitigar esta posibilidad, los paquetes se diseñan con una caducidad denominada **tiempo de vida** o **límite de saltos**. El TTL de los paquetes también puede ser útil para determinar cuánto tiempo ha estado en circulación un paquete determinado, y permite que el remitente pueda recibir información sobre la trayectoria de un paquete a través de Internet.

Cada paquete tiene un lugar en el que se almacena un valor numérico que determina cuánto tiempo debe seguir moviéndose por la red. Cada vez que un enrutador recibe un paquete, resta uno al recuento de TTL y lo pasa al siguiente lugar de la red. Si en algún momento el recuento de TTL llega a cero después de la resta, el enrutador descartará el paquete y enviará un mensaje ICMP al host de origen.

Si enviamos un paquete a una máquina y recibimos una respuesta que tiene un valor TTL de **128**, es probable que la máquina esté ejecutando **Windows**. Si recibimos una respuesta con un valor TTL de **64**, es más probable que la máquina esté ejecutando **Linux**.

A continuación, se os comparte la página que mostramos en esta clase para identificar el sistema operativo correspondiente a los diferentes valores de TTL existentes.
-   **Subin’s Blog**: [https://subinsb.com/default-device-ttl-values/](https://subinsb.com/default-device-ttl-values/)

Asimismo, os compartimos el script de Python encargado de identificar el sistema operativo en función del TTL obtenido:
-   **WhichSystem**: [https://pastebin.com/HmBcu7j2](https://pastebin.com/HmBcu7j2)