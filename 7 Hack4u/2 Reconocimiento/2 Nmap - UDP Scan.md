### Escaneo por UDP

Tags: #Nmap #Tool #Reconocimiento #Escaneo #Comandos 

❯ **nmap -p- --open  -sU ❮IP❯ -v -n -Pn** Para que nos devuelva los diferentes puertos que encuentre (Open, Filtered)
* p -> Escanea todo el rango de puertos 65535, puedes especificar un puerto a escanear asi: **-p22** etc...
* IP -> Direccion a escanear
* v -> Verbose y lo que hace es que nos muestra mas detalles al momento de ejecutar el programa
* open -> Escanea solo los puertos abiertos 
* n -> Quitamos la resolucion DNS
* Pn -> Indica que todas las direcciones son marcadas como UP
* sU -> Escanear por el protocolo UDP


❯ **nmap --top-ports 500 --open -sU ❮IP❯ -v -n -Pn** Para que nos escanee los 500 puertos mas comunes, si no se especifica, lo hace por TCP
* top-ports -> Para escanear los 500 puertos mas comunes  por UDP