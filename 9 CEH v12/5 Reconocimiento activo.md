# Reconocimiento Activo

Tags: #Kali #Linux #Tor #Nmap #THM #CEH

## Instalar Tor en Kali Linux

* Anonimato 

```bash 

❯  apt-get update && apt-get full-upgrade                          # Actualizar Kali Linux
❯  git clone https://github.com/brainfucksec/kalitorify.git        # Clonamos el repositorio de Kalitorify para tener anonimato
❯  cd kalitorify 
❯  apt install tor -y      # Instalar Tor
❯  make install 
❯  ./kalitorify.sh -h      # Para ver el panel de ayuda del kalitorify
❯  ./kalitorify.sh -v      # Para ver la version del tunel 
❯  ./kalitorify.sh -t      # Iniciamos el servicio Tor, para que nos cambie las IP Publicas (Inicia la transparencia del proxy)
❯  ./kalitorify.sh -i      # Mirar la informacion de nuestra maquina como IP publica, hostname, region, country, postal, timezone, etc...
❯  ./kalitorify.sh -s      # Mirar el estatus 
❯  ./kalitorify.sh -r      # Restart el servicio y cambia la direccion IP, no hay un limite de cambio  

# ERROR SOLUCION 
❯  Nmap localhost 
❯  Systemctl stop tor
```


## Ruta 'Nmap' en Try Hack Me 

Podemos ir haciendo rutas de THM para ir aprendiendo a usar herramientas. 
* [Nmap Live Host Discovery](https://tryhackme.com/room/nmap01)
* [Nmap Basic Port Scans](https://tryhackme.com/room/nmap02)
* [Nmap Advanced Port Scans](https://tryhackme.com/room/nmap03)
* [Nmap Post Port Scans](https://tryhackme.com/room/nmap04)


### Nmap Live Host Discovery 

Presentamos los diferentes enfoques que utiliza Nmap para descubrir hosts en vivo. En particular, cubrimos:
1. Escaneo ARP : este escaneo utiliza solicitudes ARP para descubrir hosts en vivo
2. Escaneo ICMP: este escaneo utiliza solicitudes ICMP para identificar hosts en vivo
3. Escaneo de ping TCP /UDP: este escaneo envía paquetes a puertos TCP y puertos UDP para determinar hosts en vivo.


 En términos generales, puede proporcionar una lista, un rango o una subred. Ejemplos de especificación de objetivos son:
 
- lista: `MACHINE_IP scanme.nmap.org example.com`escaneará 3 direcciones IP.
- rango: `10.11.12.15-20`escaneará 6 direcciones IP: `10.11.12.15`, `10.11.12.16`,… y `10.11.12.20`.
- subred: `MACHINE_IP/30`escaneará 4 direcciones IP.

