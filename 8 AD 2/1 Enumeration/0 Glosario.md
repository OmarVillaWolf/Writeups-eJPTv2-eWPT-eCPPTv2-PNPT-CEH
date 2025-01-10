# Glosario 

Tags: #AD #Glosario 

## CyberKillChain recomendado en AD

```bash 
1. Enumeracion local del equipo 
2. Escalacion de privilegios locales (Si aplica)
3. Apagar Windows Defender
4. Exfiltrar las credenciales con Mimikatz
5. Realizar un Password Spraying (opcional)
6. Realiza un reporte informal que contenga lo mas importante (hashes, contraseñas, usuarios, etc)
```

## Principales conceptos en AD

```bash 

1. Dominio (Domain)
- Definición: Es una entidad lógica dentro del Directorio Activo que agrupa un conjunto de objetos bajo una única política de seguridad y base de datos.
- Ejemplo: 'domain1.com' podría ser el nombre de dominio principal de una organización, bajo el cual todos los usuarios, equipos y recursos están registrados.
    
2. Objeto (Object)
- Definición: Es una entidad individual dentro del Directorio Activo, como un usuario, grupo o equipo.
- Ejemplo: 'john.doe@domain1.com' podría ser un objeto usuario en el dominio.
    
3. Árbol (Tree)
- Definición: Es una colección de uno o más dominios de Directorio Activo que comparten un espacio de nombres contiguo y una estructura jerárquica. 
- Ejemplo: Bajo el dominio 'domain1.com', podría haber subdominios como 'sales.domain1.com' y 'research.domain1.com'. Juntos, forman un árbol.
    
4. Bosque (Forest)
- Definición: Es una colección de árboles que comparten un catálogo global, esquema y operaciones maestras. Cada bosque actúa como un contenedor de seguridad.
- Ejemplo: 'domain1.com' y 'domain2.com' podrían ser dos árboles diferentes pero pertenecer al mismo bosque, compartiendo un catálogo global y políticas de seguridad.
    
5. Unidad Organizativa (OU)
- Definición: Es un contenedor dentro del Directorio Activo que puede contener usuarios, grupos, equipos y otras OUs. Ayuda a organizar y administrar recursos.
- Ejemplo: Dentro de 'domain1.com', podría haber una OU llamada "Departamento de Finanzas" que agrupa a todos los usuarios y recursos relacionados con las finanzas.
    
6. ACL (Access Control List)
- Definición: Es una lista de permisos adjuntos a un objeto que especifica qué usuarios/grupos tienen acceso al objeto y qué operaciones pueden realizar.
- Ejemplo: En la OU "Departamento de Finanzas" de 'domain1.com', podría haber una ACL que solo permita a los usuarios del departamento acceder a ciertos archivos críticos.
    
7. GPO (Group Policy Object)
- Definición: Es un conjunto de configuraciones de políticas que se pueden aplicar a usuarios o equipos dentro de un dominio. Las GPOs controlan una variedad de configuraciones, desde políticas de seguridad hasta configuraciones de software.
- Ejemplo: Una GPO en 'domain1.com' podría establecer que todas las computadoras del dominio tengan actualizaciones automáticas activadas y que se bloqueen después de 10 minutos de inactividad.
```

## Componentes Principales:

```bash 
1. Key Distribution Center (KDC): Es una instancia de confianza centralizada que provee los servicios AS y TGS.
2. Authentication Service (AS): Es el servicio dentro del KDC que autentica a los usuarios y servicios y emite TGTs.
3. Ticket Granting Service (TGS): Es otro servicio dentro del KDC que emite tickets de servicio para acceder a recursos después de que el cliente ha sido autenticado inicialmente por el AS.
4. Ticket Granting Ticket (TGT): Es un "boleto" que el AS emite a los clientes cuando se autentican con éxito, que a su vez es utilizado para solicitar tickets de servicio del TGS.
```

## Usuarios predeterminados de Active Directory:

```bash 
1. Administrator: La cuenta de administrador del dominio que tiene acceso total a todos los servidores y estaciones de trabajo del dominio.
2. Guest: Una cuenta de usuario que tiene privilegios limitados, generalmente deshabilitada por defecto.
```