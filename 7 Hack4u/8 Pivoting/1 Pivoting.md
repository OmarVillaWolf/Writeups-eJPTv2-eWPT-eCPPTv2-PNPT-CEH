# Pivoting

Tags: #Pivoting 

El **pivoting** (también conocido como “hopping”) es una técnica utilizada en pruebas de penetración y en el análisis de redes que implica el uso de una máquina comprometida para atacar otras máquinas o redes en el mismo entorno.

Por ejemplo, si un atacante ha comprometido una máquina en una red corporativa, puede utilizar técnicas de pivoting para utilizar esa máquina como punto de salto para atacar otras máquinas en la misma red que de otra manera no serían accesibles. Esto se logra a través de la creación de túneles de comunicación desde la máquina comprometida a otras máquinas en la red.

El pivoting puede ser utilizado para superar restricciones de seguridad que de otra manera impedirían a un atacante acceder a determinadas máquinas o redes. Por ejemplo, si una red corporativa utiliza segmentación de red para separar diferentes partes de la red, el pivoting puede ser utilizado para superar esta restricción y permitir que un atacante salte de una red a otra.

## Descubrimiento de IPs

```bash 
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

## Descubrimiento de Puertos 

```bash
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

