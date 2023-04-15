# Bash ❮❯

Tags: #Comandos #Shell 

## Aplicar Filtros 

Cuando recibimos un output, podamos mostrar solo el segundo argumento 
```bash
❯ cat file | awk '{print $2}' FS=":"

	# awk = 
	# FS = El delimitador son los :
```
