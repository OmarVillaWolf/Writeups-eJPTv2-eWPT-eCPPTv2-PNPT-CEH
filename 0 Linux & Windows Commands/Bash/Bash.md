# Bash ❮❯

Tags: #Comandos #Shell 

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

Por si el comando ping no esta disponible
```bash
❯ echo ‘’ > /dev/tcp/172.16.0.1/22) 2>/dev/null && echo “[+] Puerto abierto” || echo “[+] Puerto cerrado”
```

