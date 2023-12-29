# Bash 

Tags: #Bashshell 

```bash 
❯ bash script.sh            # Ejecutar un script sin permisos de ejecusión
❯ ./script.sh               # Ejecutar un script con permisos de ejecusión 
```

## Obtener la IP 

```bash 
#!/bin/bash 

# Agregar Colores
greenColour="\e[0;32m\033[1m"
endColour="\033[0m\e[0m"
redColour="\e[0;31m\033[1m"
blueColour="\e[0;34m\033[1m"
yellowColour="\e[0;33m\033[1m"
purpleColour="\e[0;35m\033[1m"
turquoiseColour="\e[0;36m\033[1m"
grayColour="\e[0;37m\033[1m"

echo -e "\n${yellowColour}[+]${endColour} Esta es tu direccion IP privada -> $(ip a | grep ens33 | tail -n 1 | awk '{print $1}' FS="/")\n"
	# e = Interpretar caracteres especiales (\n = salto de linea), colores.
	# Cada color que se quiera agregar, se debe de abrir y cerrar 
```
