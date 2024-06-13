# PowerView 

Tags: #AD #Powershell #Enumeracion #Windows 



```bash 
❯ powershell -ep bypass                      # Politica que nos permite ejecutar scripts en Powershell
 	# ep = Ejecutar politicas 


❯ . .\PowerView.ps1                          # Importamos el modulo
❯ Get-Domain                                 # Miramos el dominio 'FOREST'
❯ Get-Domain -Domain <Forest.name>           # Miramos informacion acerca de 'Parent domain'

❯ Get-DomainSID                              # Muestra el SID del dominio 

❯ Get-DomainPolicy                           # Muestra las politicas del dominio (KerberosPolicy, SystemAccess)
	(Get-DomainPolicy)."SystemAccess" 
	(Get-DomainPolicy)."KerberosPolicy"

❯ Get-DomainController                       # Muestra informacion del controlador de dominio (Roles, Domain, IP, Forest)
	Get-DomainController -Domain <Forest.name>

❯ Get-DomainUser                             # Muestra informacion de los usuarios 
```
