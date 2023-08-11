# Escaneo con Metasploit 

## Reconocimiento en la red local con ARP 

```bash 
❯ use auxiliary/scanner/discovery/arp_sweep      # Para usar el exploit
❯ options                                        # Miramos las opciones del exploit y ver lo que es necesario cargar para hacer el barrido en la subred
❯ info 'o' info -d                               # Nos da una breve descripcion de lo que hace el exploit
❯ set RHOSTS 192.168.68.0/24                     # Colocamos la subred en donde queremos hacer el barrido del ARP
❯ run
❯ hosts                                          # Nos muestra los hosts que ha descubierto en el reconocimiento
❯ services                                       # Nos muestra los servicios, pero para que sea mas efectivo, debemos de pasarle el archivo XML de Nmap
```