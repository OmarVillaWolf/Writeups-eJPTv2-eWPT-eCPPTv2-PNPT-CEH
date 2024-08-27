# Bash 

Tags: #Bashshell #Regex

```bash 
❯ bash script.sh            # Ejecutar un script sin permisos de ejecución
❯ ./script.sh               # Ejecutar un script con permisos de ejecución 
```

## REGEX

```bash
# Cuando recibimos un output, podemos mostrar solo el segundo argumento 

❯ cat file | awk '{print $2}' FS=":"

	# awk = # Al momento de filtrar, le podemos decir que nos queremos quedar con el segundo argumento
	# FS = El delimitador son los ':'
```

```bash
❯ awk '{print $1}' FS="<"         # Listar el primer argumento (izquierda a derecha), tomando el simbolo '<' como delimitador 
```

```bash
❯ awk 'NF{print $NF}'             # Nos quedamos con el ultimo argumento
```

```bash
❯ sed 's/^ *//'                   # Quitamos el espaciado al principio de un otuput

	# Todo lo que inicie con un espacio seguido de 'nada', lo quite 
```

```bash 
❯ grep -oP '^amor.*' /usr/share/wordlists/rockyou.txt     # Listamos la palabra 

	# ^ = Indicamos que inicie con la palabra 'amor'
	# .* = Indicamos que termine con lo que sea

❯ grep -oP '.*amor$' /usr/share/wordlists/rockyou.txt     # Listamos la palabra 

	# .* = Indicamos que inicie con lo que sea 
	# $ = Indicamos que finalice con la palabra 'amor'
```