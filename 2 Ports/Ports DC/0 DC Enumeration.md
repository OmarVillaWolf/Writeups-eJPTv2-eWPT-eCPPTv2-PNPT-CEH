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
	Enumeración: 'Ldapsearch' 'Ldapdomaindump' 'GetADUsers'

❯ Puerto 445 SMB:
	Enumeración: 'Nextec / Crackmapexec' 'SMBClient' 'SMBMap' 

❯ Puerto 1433 MsSQLServer:
	Conexión: 'Impacket-mssqlclient'

❯ Puerto 5985, 5986 WinRM:
	Conexión: 'Evil-winrm'

Tips: 
	1. Descubir puertos de un DC = 88 Kerberos,389 LDAP, 636 LDAPS, 3268 GC  
	2. Buscar el nombre del dominio y agregarlo al archivo '/etc/hosts' DC01.domain1.local domain1.local obteniendolo con la herramienta 'Nextec'
	3. Enumeración (Shares, Usuarios) 'https://app.snov.io/login'
	4. Si solo hay usuarios validos se puedo solicitar un TGT con un 'ASREProast attack'
	5. Si ya hay credenciales validas se puede obtener un TGS con 'Kerberoasting attack'
```

## Active Directory 

```bash 
Metodología en CRTA

Nota:
1. Enumeración de la red para comprometer una maquina vulnerable 
2. Si existe otro segmento de red en la maquina vulnerable hacer 'pivoting' (En caso de aplicar) 
3. Enumeración local en la segunda maquina vulnerada para obtener permisos de 'Administrator', ademas de, acceder con el usuario que se encuentra en el dominio del DC 
4. Identificar las maquinas en el dominio con 'Find-WMILocalAdminAccess.ps1 -Verbose'
```