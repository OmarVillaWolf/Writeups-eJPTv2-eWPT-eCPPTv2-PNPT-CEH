# Pass The Hash 

Tags: #AD #Window #PassTheHash 

PtH es una técnica donde robas una credencial principalmente en los sistemas Windows. Un atacante puede obtener los hashes de las password de un usuario y usarlo para autenticarse como ese usuario, bypaseando la necesidad de la actual password en texto plano. Este ataque principalmente comienza cuando el atacante gana acceso no autorizado a un sistema comprometido donde los hashes de las password estan almacenadas. Una vez adquiridas, el atacante puede explotar la vulnerabilidad en el protocolo de autenticacion de Windows, como un NTLM o Kerberos, para pasar las credenciales hasheadas a otros sistemas dentro del dominio de AD.  Para 'bypassing' se necesita crackear la password, el atacante puede moverse lateralmente dentro de la red, escalar privilegios y potencialmente hacer actividades maliciosas.  

```bash 
❯ 
```

```bash 
❯ powershell -ep bypass                      # Politica que nos permite ejecutar scripts en Powershell
 	# ep = Ejecutar politicas 
 	
❯ . .\PowerView.ps1                          # Importamos el modulo
❯ Get-Domain 
❯ Find-LocalAdminAccess                      # Nos mostrara el 'seclogs' para agregarlo en el siguiente comando 

❯ Enter-PSSession seclogs.domian.local       # Ingresamos a una sesion 
	❯ whoami /all                           # Muestra los privilegios asociados con el admin 
	❯ 
```