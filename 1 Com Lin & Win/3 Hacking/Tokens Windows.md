# Tokens de acceso 

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

# Privilegios en Windows

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
12. [SeImpersonatePrivilege: Impersonar a un cliente después de la autenticación.
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