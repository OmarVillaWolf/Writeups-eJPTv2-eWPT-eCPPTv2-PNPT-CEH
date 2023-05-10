## Técnicas de evasión de Firewalls / IDS y Spoofing (MTU, Data Length, Source Port, Decoy, etc.)

Tags: #Nmap #Tool #Reconocimiento #Escaneo #Comandos 

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