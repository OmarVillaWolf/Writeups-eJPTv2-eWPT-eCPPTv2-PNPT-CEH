# IPV4 e IPV6 ❮❯

Tags: #Subnetting #Redes #OSI #Puerto

* Las direcciones IP tienen 4 octetos
* 1 byte = 8 bits

❯ **echo "$(echo "obase=2, 192" | bc).168.111.42"** Nos cambiara el 192 a codigo binario y nos lo moestrara por pantalla  
* 11000000 = 192
* 10101000 = 168
* 1101111 = 111
* 101010 = 42

La parte de Subnetting lo tenemos en la hoja de Calculo de Excel en Google.

## Direcciones MAC (OUI y NIC)

Direccion MAC son direcciones fisicas y son unicas para cada dispositivos, se representa de la siguiente manera:
* 00:0C:29:e1:6d:92 
* Las direcciones MAC tienen 6 bytes 

OUI (Organization Unic Identifier) = 00:0C:29 -> Siempre es la primer parte de la MAC y con eso podemos detectar que tipo de dispositivo es
NIC (Network Interface Controller) = e1:6d:92 -> Siempre la segunda parte de la MAC

❯ **arp-scan -I ens33 --localnet** Para hacer un escaneo de la red (I=i mayuscula) en la interface ens33
❯ **macchanger -l** Podemos listar direcciones MAC (l=ele)
	❯ **macchanger -l | grep -i vmware** Podemos listar direcciones MAC (l=ele), y filtramos por el vendor que querramos


## Protocolos comunes (UDP, TCP) y el famoso Three-Way Handshake
Los protocolos TCP (Transmission Control Protocol) y UDP (User Datagram Protocol) son dos de los protocolos de red más comunes utilizados en la transferencia de datos a través de redes de ordenadores.

* Tenemos **65535** puertos 

El protocolo TCP, es un protocolo orientado a la conexión que proporciona una entrega de datos confiable, mientras que el protocolo UDP, es un protocolo no orientado a conexión el cual no garantiza la entrega de datos.

Una parte crucial del protocolo TCP es el Three-Way Handshake, un procedimiento utilizado para establecer una conexión entre dos dispositivos. Este procedimiento consta de tres pasos: SYN, SYN-ACK y ACK, en los que se sincronizan los números de secuencia y de reconocimiento de los paquetes entre los dispositivos. El Three-Way Handshake es fundamental para establecer una conexión confiable y segura a través de TCP.

**Puertos TCP comunes:**
* 21: FTP (File Transfer Protocol) – permite la transferencia de archivos entre sistemas.
* 22: SSH (Secure Shell) – un protocolo de red seguro que permite a los usuarios conectarse y administrar sistemas de forma remota.
* 23: Telnet – un protocolo utilizado para la conexión remota a dispositivos de red.
* 80: HTTP (Hypertext Transfer Protocol) – el protocolo que se utiliza para la transferencia de datos en la World Wide Web.
* 443: HTTPS (Hypertext Transfer Protocol Secure) – la versión segura de HTTP, que utiliza encriptación SSL/TLS para proteger las comunicaciones web.

**Puertos UDP comunes:**
* 53: DNS (Domain Name System) – un sistema que traduce nombres de dominio en direcciones IP.
* 67/68: DHCP (Dynamic Host Configuration Protocol) – un protocolo utilizado para asignar direcciones IP y otros parámetros de configuración a los dispositivos en una red.
* 69: TFTP (Trivial File Transfer Protocol) – un protocolo simple utilizado para transferir archivos entre dispositivos en una red.
* 123: NTP (Network Time Protocol) – un protocolo utilizado para sincronizar los relojes de los dispositivos en una red.
* 161: SNMP (Simple Network Management Protocol) – un protocolo utilizado para administrar y supervisar dispositivos en una red.


## El modelo OSI – ¿En qué consiste y cómo se estructura la actividad de red en capas?
En redes de ordenadores, el modelo OSI (Open Systems Interconnection) es una estructura de siete capas que se utiliza para describir el proceso de comunicación entre dispositivos. Cada capa proporciona servicios y funciones específicas, que permiten a los dispositivos comunicarse a través de la red.

A continuación, se describen brevemente las siete capas del modelo OSI:

* Capa física: Es la capa más baja del modelo OSI, que se encarga de la transmisión de datos a través del medio físico de la red, como cables de cobre o fibra óptica.
* Capa de enlace de datos: Esta capa se encarga de la transferencia confiable de datos entre dispositivos en la misma red. También proporciona funciones para la detección y corrección de errores en los datos transmitidos.
* Capa de red: La capa de red se ocupa del enrutamiento de paquetes de datos a través de múltiples redes. Esta capa utiliza direcciones lógicas, como direcciones IP, para identificar dispositivos y rutas de red.
* Capa de transporte: La capa de transporte se encarga de la entrega confiable de datos entre dispositivos finales, proporcionando servicios como el control de flujo y la corrección de errores.
* Capa de sesión: Esta capa se encarga de establecer y mantener las sesiones de comunicación entre dispositivos. También proporciona servicios de gestión de sesiones, como la autenticación y la autorización.
* Capa de presentación: La capa de presentación se encarga de la representación de datos, proporcionando funciones como la codificación y decodificación de datos, la compresión y el cifrado.
* Capa de aplicación: La capa de aplicación proporciona servicios para aplicaciones de usuario finales, como correo electrónico, navegadores web y transferencia de archivos.
