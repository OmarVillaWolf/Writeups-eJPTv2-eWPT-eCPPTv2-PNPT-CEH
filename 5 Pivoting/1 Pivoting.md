# Pivoting

Tags: #Pivoting 

El **pivoting** (también conocido como “hopping”) es una técnica utilizada en pruebas de penetración y en el análisis de redes que implica el uso de una máquina comprometida para atacar otras máquinas o redes en el mismo entorno.

Por ejemplo, si un atacante ha comprometido una máquina en una red corporativa, puede utilizar técnicas de pivoting para utilizar esa máquina como punto de salto para atacar otras máquinas en la misma red que de otra manera no serían accesibles. Esto se logra a través de la creación de túneles de comunicación desde la máquina comprometida a otras máquinas en la red.

El pivoting puede ser utilizado para superar restricciones de seguridad que de otra manera impedirían a un atacante acceder a determinadas máquinas o redes. Por ejemplo, si una red corporativa utiliza segmentación de red para separar diferentes partes de la red, el pivoting puede ser utilizado para superar esta restricción y permitir que un atacante salte de una red a otra.

## Escaneo en Linux

```bash 
1. Descubrimiento de IPs desde una maquina Linux

#!/bin/bash 

function ctrl_c(){
	echo -e "\n\n[!] Saliendo...\n"
	tput cnorm; exit 1
}

# Ctrl_c
trap ctrl_c INT

tput civis

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 10.10.0.$i" &>/dev/null && echo "[+] Host 10.10.0.$i - ACTIVE" &
done; wait

tput cnorm
```

```bash
1.  Descubrimiento de puertos desde una maquina Linux 

#!/bin/bash 

function ctrl_c(){
	echo -e "\n\n[!] Saliendo...\n"
	tput cnorm; exit 1
}

# Ctrl_c
trap ctrl_c INT

tput civis

for port in $(seq 1 65535); do
	timeout 1 bash -c "echo '' > /dev/tcp/10.10.0.0/$port" 2>/dev/null && echo "[+] PORT $port - OPEN" & 
done; wait 

tput cnorm
```

## Escaneo en Windows 

* [Escaneo de IPs](https://github.com/BornToBeRoot/PowerShell_IPv4NetworkScanner/tree/main/Scripts)

```bash 
1. Descubrimiento de IPs desde una maquina Windows con ARP
❯ arp -a       # Mirar la tabla de ARP 

# Descubrimiento de IPs desde una maquina Windows con la herramienta
1.Descargar la tool en Kali para despues transferirla a la máquina víctima con Windows
❯ wget https://raw.githubusercontent.com/BornToBeRoot/PowerShell_IPv4NetworkScanner/refs/heads/main/Scripts/IPv4NetworkScan.ps1

# Ejecutar en Windows
❯ .\IPv4NetworkScan.ps1 -StartIPv4Address 10.10.10.0 -EndIPv4Address 10.10.10.254   # Escanear con la herramienta el segmento de red en busca de IPs activas  
```

```bash 
1 Descubrimiento de puertos desde una maquina Windows teniendo un túnel ya activado con 'Proxychains' el cual hace que se pueda escanear los puertos desde Kali

❯ ❯ seq 1 65535 | xargs -P 500 -I {} proxychains nmap -sT -Pn -p{} -open -T5 -v -n ❮Target IP❯ --append-output -oG allPorts 2>&1 | grep -vE "chain|Initiating|Starting|timeout|seconds|Read|Completed|Scanning"
```