## Nmap y sus diferentes modos de escaneo

Tags: #Nmap #Tool #Reconocimiento #Escaneo #Comandos 

### Escaneo por TCP
```bash 
❯ nmap -sn ❮IP/24❯                            # Aplica un barrido con ping, para ver por trazas ICMP ver si el equipo esta activo
	# IP = Direccion para hacer el barrido y debe terminar con .0/24

# Aplica un barrido con ping y solo mostrara las IP que encuentre y las ordenara
❯ nmap -sn ❮IP/24❯ | grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}' | sort  

❯ nmap -p- --open -T5 -sT ❮IP❯ -v -n -Pn      # Para que nos devuelva los diferentes puertos que encuentre (Open, Filtered)
	# p -> Escanea todo el rango de puertos 65535, puedes especificar un puerto a escanear asi: -p22 etc...
	# IP -> Direccion a escanear
	# v -> Verbose y lo que hace es que nos muestra mas detalles al momento de ejecutar el programa
	# open -> Escanea solo los puertos abiertos 
	# n -> Quitamos la resolucion DNS
	# T -> Nos permite controlar la plantilla de temporisado (0,1,2,,4,5)
	# sT -> Manda un SYN -> (RST (Cerrado) | SYN/ACK (Abierto)) -> ACK (Established))
	# Pn -> Indica que todas las direcciones son marcadas como UP

❯ nmap -p22,80 -sVC ❮IP❯                      # Para ver la Version y el Servicio que corren esos puertos (Fingerprinting)
	# sV -> Detecta la version y el servicio que estan corriendo el puerto seleccionado
	# sC -> Para lanzar un conjunto de scripts basicos de reconocimiento para que nos enumere mas informacion adicional

❯ nmap --top-ports 500 ❮IP❯                   # Para que nos escanee los 500 puertos mas comunes, si no se especifica, lo hace por TCP
	# top-ports -> Para escanear los 500 puertos mas comunes  

❯ nmap -O ❮IP❯                                # Para detectar el OS (Pero no es recomendable)
	# O -> Detecta cual es el sistema operativo
```