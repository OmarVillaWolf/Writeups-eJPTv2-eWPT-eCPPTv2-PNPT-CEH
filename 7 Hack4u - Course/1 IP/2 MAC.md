## Direcciones MAC (OUI y NIC)

Tags: #Redes #MAC

Direccion MAC son direcciones fisicas y son unicas para cada dispositivos, se representa de la siguiente manera:
* 00:0C:29:e1:6d:92 
* Las direcciones MAC tienen 6 bytes 

OUI (Organization Unic Identifier) = 00:0C:29 -> Siempre es la primer parte de la MAC y con eso podemos detectar que tipo de dispositivo es
NIC (Network Interface Controller) = e1:6d:92 -> Siempre la segunda parte de la MAC

❯ **arp-scan -I ens33 --localnet** Para hacer un escaneo de la red (I=i mayuscula) en la interface ens33
❯ **macchanger -l** Podemos listar direcciones MAC (l=ele)
	❯ **macchanger -l | grep -i vmware** Podemos listar direcciones MAC (l=ele), y filtramos por el vendor que querramos