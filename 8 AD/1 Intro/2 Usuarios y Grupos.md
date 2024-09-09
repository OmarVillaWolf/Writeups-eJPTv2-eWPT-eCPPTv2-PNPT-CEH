# Usuarios y grupos

Tags: #AD 

**Usuarios:** Son individuales e interactuan con la red. Cada usuario tiene una única cuenta dentro AD, identificada por un usuario y asociada a una contraseña. Los usuarios usan sus credenciales para hacer 'log' en sus computadoras, acceder a recursos de red y ejecutar varias tareas. Las cuentas de usuarios en AD pueden almacenar información como su nombre completo, email, numero telefónico, titulo y departamento. Esta información puede ser usada para la autenticación, autorización y propósitos de manejo.  Los administradores pueden manejar las cuentas de usuario creando, modificando o borrandolas usando las herramientas de AD. Ellos también pueden asignar permisos, membresias de grupo y otras configuraciones para controlar el acceso a los recursos de red.  

**Grupos:** Coleccionan las cuentas de usuarios, computadoras y otros grupos de AD. Proveen una manera conveniente para manejar los permisos de acceso y aplicar las configuraciones a múltiples usuarios o computadoras simultáneamente. Existen dos grupos principales:
* **Grupos de seguridad:** Son usados para manejar los permisos de acceso a recursos de red. Los usuarios pueden ser agregados a grupos de seguridad y los permisos pueden ser asignados a esos grupos para controlar los recursos de acceso. 
* **Distribución de grupos:** Son usados para enviar mensajes de email a recipientes de grupos. No tiene permisos de seguridad y principalmente son usados para propósitos de distribución de email. 

**Grupos de seguridad:** 
* Administradores de grupo: Es uno de los grupos mas poderosos en AD. Es automáticamente creado  cuando el primer dominio es instalado y tiene un control administrativo sobre el dominio entero. 
* Administradores Enterprise: Son un grupo de seguridad de todo el bosque que tiene privilegios administrativos sobre todos los dominios dentro de AD. 
* Operadores de servidores: Es un grupo de seguridad con permisos para administrar controladores de dominio y servidores miembros dentro del dominio. 
* Operadores de respaldo: Es un grupo de seguridad con permisos para realizar copias de seguridad y restaurar operaciones en controladores de dominio y servidores miembro. 
* Operadores de cuentas: Es un grupo de seguridad con permisos para administrar cuentas de usuarios, grupos y cuentas de equipo dentro del dominio. 
* Usuarios de dominio: Son cuentas individuales creadas dentro del  AD para representar personas que interactuan con la red. A cada usuario se le asigna un nombre de usuario único y contraseña, que usan para iniciar sesión en computadoras unidas al dominio y acceder a los recursos de red. 
* Computadoras de dominio: Son dispositivos, incluidas estaciones de trabajo, computadoras portátiles, servidores y dispositivos de red que están unidos al dominio de AD. Cuando se conecta un ordenador al dominio, establece una relación de confianza con el dominio y se convierte en miembro del dominio. 
* Controladores de dominio (DC): Es un servidor que opera dentro del entorno de AD y es responsable de autenticar a los usuarios, autorizar el acceso a los recursos y mantenimiento de la base de datos del directorio.

