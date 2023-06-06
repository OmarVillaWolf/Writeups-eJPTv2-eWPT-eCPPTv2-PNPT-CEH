# Bash

Tags: #Comandos #BashShell 

## Aplicar Filtros 

Cuando recibimos un output, podamos mostrar solo el segundo argumento 
```bash
❯ cat file | awk '{print $2}' FS=":"

	# awk = # Al momento de filtrar, le podemos decir que nos queremos quedar con el segundo argumento
	# FS = El delimitador son los ':'
```

```bash
❯ awk '{print $1}' FS="<"                   # Listame el primer argumento (izquierda a derecha), tomando el simbolo '<' como delimitador 
```

```bash
❯ awk 'NF{print $NF}'                       # Al momento de filtrar, le podemos decir que nos queremos quedar con el ultimo argumento
```

```bash
❯ sed 's/^ *//'                             # Quitamos el espaciado al principio de un otuput

	# Queremos que todo lo que inicie con espacio seguido de nada, me lo quite 
```

Para ir descubriendo interfaces con el comando ping 
```bash 
!#/bin/bash 

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 10.10.0.$i" &>/dev/null && echo "[+] Host 10.10.0.$i - ACTIVE" &
done; wait  
```

Por si el comando ping no esta disponible, podemos hacerlo por los puertos mas comunes 
```bash
!#/bin/bash 

for i in $(seq 1 254); do
	for port in 21 22 80 443 445 8080; do
		timeout 1 bash -c "echo '' > /dev/tcp/10.10.0.$i/$port" &>/dev/null && echo "[+] Host 10.10.0.$i - PORT $port - OPEN" &
	done 
done; wait 
```

