# Instalar Tor en Kali Linux

Tags: #Kali #Linux #Tor

```bash 

❯  apt-get update && apt-get full-upgrade                          # Actualizar Kali Linux
❯  git clone https://github.com/brainfucksec/kalitorify.git        # Clonamos el repositorio de Kalitorify
❯  cd kalitorify 
❯  apt install tor -y 
❯  make install 
❯  ./kalitorify.sh -h 
❯  ./kalitorify.sh -v 
❯  ./kalitorify.sh -t 

# ERROR SOLUCION 
❯  Nmap localhost 
❯  Systemctl stop tor
```