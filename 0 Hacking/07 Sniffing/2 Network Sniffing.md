# Sniffing de red 

Tags: #Hacking #Sniffing #Wireshark 

## Wireshark 

```bash 
# Windows Tool 
❯ wireshark &       # Para snifear el tráfico en la interface 

Filtros:
❯ http.request.method eq POST       # Filtrar por el método POST 
	1. Ir a la opción 'Edit > Find Packet' para agregar un nuevo filtro 
	2. Seleccionar 'String, Packet Details y Narrow(UTF8/ASCII)' y colocar 'pwd' 
	3. Dar click en 'Find'

1. Tambien se puede dare click derecho 'Follow > TCP Stream' y se mostrará el string completo entre la máquina víctima y el sitio web 
2. Tambien se puede aplicar filtros 
```

## Omnipeek Network Protocol Analyzer

```bash 
# Web Tool 
❯ https://www.liveaction.com/products/omnipeek-network-protocol-analyzer/   

Nota:
	1. Dar click en 'Start Trial', hacer el registro para descargar el instalador y al llave de licencia  
	2. Ejecutar el instalador y seleccionar 'Automatic: requires an internet connection' e instalar 
	3. Dar click en 'New Capture', en 'Capture Options' seleccionar el adaptador 
	4. Dar click en la opción 'Start'
```