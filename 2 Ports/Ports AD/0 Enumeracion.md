# Enumeración del DC

Tags: #AD #Enumeracion #Puerto #DC 

```powershell
❯ Puerto 53 DNS:
	Enumeración: 'Dig'

❯ Puerto 88 Kerberos: 
	Enumeración: 'Kerbrute' 
	
❯ Puerto 135 RPC: 
	Enumeración: 'RPCClient'

❯ Puerto 389 LDAP:
	Enumeración: 'ldapdomaindump'

❯ Puerto 445 SMB:
	Enumeración: 'Nextec / Crackmapexec' 'SMBClient' 'SMBMap' 

❯ Puerto 1433 MsSQLServer:
	Conexión: 'Impacket-mssqlclient'

❯ Puerto 5985, 5986 WinRM:
	Conexión: 'Evil-winrm'

Tips: 
	1. Agregar el nombre de la maquina y su dominio al archivo '/etc/hosts' DC01.domain1.local domain1.local obteniendolos con la herramienta 'Nextec'
	2. Si solo hay usuarios validos se puedo solicitar un TGT con un 'ASREProast attack'
	3. Si ya hay credenciales validas se puede obtener un TGS con 'Kerberoasting attack'
```
