# Unidades Organizacionales (OUs)

Tags: #AD #Kerberos #NTLM

## OUs

OUs pueden contener usuarios, grupos, computadoras y otros OUs, creando un estructura jerárquica que refleja la estructura de la organización.  Pueden tener Política de Objetos de Grupo (GPOs) enlazados para aplicar configuraciones especificas a objetos dentro de la OU. Ademas, tiene tareas administrativas como delegar el control y permisos de configuración, estos pueden ser asignados a específicos OUs para distribuir responsabilidades administrativas. Pueden ser las diferentes áreas que se encuentran en una organización. 

**GPO** = Site -> Domain -> OU

## Autenticación 

Es el proceso por el cual los usuarios y computadoras verifican sus identidades para ganar acceso a los recursos de red dentro del AD. Juega un rol critico asegurando el acceso de seguridad a recursos mientras hace un proceso centralizado de control a través de la autenticación y manejo de acceso. La mejor forma de que los usuarios se autentiquen es cuando proveen un usuario y una contraseña. 

AD soporta severos protocolos de autenticación que facilitan la comunicación segura y la autenticación del usuario dentro del entorno de AD. Esos protocolos juegan un rol crucial asegurando la confidencialidad, integridad y autenticidad. Algunos protocolo clave de AD son los siguientes:
* Kerberos
* NTLM

## Kerberos

Es el protocolo de autenticación primario usado por AD para la autenticación del usuario y trabaja de la siguiente manera:
* Cuando un usuario hace 'log in', su computadora solicita un 'Ticket Granting Ticket (TGT)' desde la 'Key Distribution Center (KDC)', el cual es tipicamente un controlador de dominio. 
* El controlador de dominio verifica que las credenciales del usuario y TGT si la autenticación es correcta. 
* El TGT después es usado para obtener 'Service Tickets' y así tener acceso a recursos específicos de la red. 

Características: 
* Autenticación mutua: Tanto el cliente como el servidor verifican las entidades del otro. 
* Single Sign-On (SSO): Los usuarios se autentican una vez y pueden acceder a múltiples recursos sin re-ingresar sus credenciales.  
* Basado en Ticket: La autenticación intercambia tickets encriptados  confiables para reducir el riesgo de credenciales robadas. 

## NTLM 

Es un protocolo de autenticación viejo (legacy) usado por los sistemas Windows  para compatibilidad y trabaja de la siguiente manera: 
* Cuando un usuario se autentica, su computadora cliente envía una versión hasheada de su contraseña a el servidor. 
* El servidor compara el hash con los hashes almacenados de la contraseña del usuario. 
* Si el hash hace 'match', la autenticación es correcta. 

Características: 
* Compatibilidad: NTLM es soportado por versiones viejas de los sistemas de Windows y aplicaciones. 
* Simplicidad: La autenticación no requiere de la complejidad de Kerberos, haciéndolo mas fácil para implementarlo en ciertos entornos.
* Seguridad: Tiene seguridad limitada comparada con Kerberos, incluyendo la susceptibilidad de los ataques de 'pass-the-hash' y falta de autenticación mutua. 