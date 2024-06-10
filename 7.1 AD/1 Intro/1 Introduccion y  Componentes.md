# Componentes de un Active Directory (AD)

Tags: #AD 

Active Directory es un servicio desarrollado por Microsoft para redes de dominio de Windows. Funciona como un servicio centralizado para manejar, organizar información acerca de los recursos de red, como usuarios, computadoras, grupos y otros dispositivos. Active Directory provee un amplio rango de servicios para identificar y manejar accesos dentro de un entorno de red. 

**Autenticación y autorización de usuarios:**  AD funciona como un mecanismo de autorización y autenticación. Los usuarios pueden hacer log dentro de sus computadoras u otros recursos de red usando sus credenciales de AD, sus permisos de acceso son controlados basado en su rol y su membresia de grupo. 

**Manejo de recurso:** Es activado por administradores para un manejo eficiente y organizar los recursos de red como computadoras, impresoras, folders compartidos y aplicaciones. Este manejo centralizado simplifica las tareas como desarrollo de software, manejo de configuraciones y control de acceso. 

**Manejo de políticas de grupo:** AD permite a los administradores definir políticas de seguridad, configuraciones a través de todos los dominios ingresados. Las políticas de grupo aseguran consistencia y  son conformadas en toda la red. 

**Servicios de directorio:** Son provee-idos por una estructura jerárquica para organizar los objetos dentro de la red, haciéndolo mas fácil para localizar y manejar recursos. Esta estructura incluye dominios, arboles, bosques, permitiendo a las organizaciones escalar sus infraestructuras de red como sean necesarias. 

**Dominios:** Un dominio es un grupo lógico de objetos de red (como computadoras, usuarios y recursos) que comparten una base de datos como directorio común y políticas de seguridad.  Los dominios son administrados como una sola unidad y forma los bloques de 'core' de un entorno de AD. 

**Controladores de dominio:** Son servidores que manejan el acceso a los recursos dentro del dominio. Ellos almacenan una replica de una base de datos de AD y autentica a los usuarios, fortalece las políticas de seguridad y replica los cambios de un controlador de dominio dentro del dominio.

**Bosque (Forest):** Es una colección de uno o mas dominios que comparten un esquema común, configuración y un catalogo global. Representa el contenedor top-level en una jerarquía de AD y define los limites de las relaciones establecidas. 

**Unidades Organizacionales (OUs):**  Son contenedores dentro de un domino que permite a los administradores organizar y manejar objetos mas eficientemente. OUs pueden ser usados para delegar tareas administrativas, aplicar políticas de grupo y controlar permisos de acceso. 

**Catalogo global:** Es un repositorio de data distribuido que contiene una replica parcial de todos los objetos en un bosque. Eso facilita a las búsquedas de cross-domain y a los usuarios activos para localizar recursos a través de todo el AD. 

**Relaciones verdaderas:** Define como la autenticación y la autorización son extendidas entre los dominios dentro de un bosque o dentro de bosques separados. Trust permite a los usuarios en un dominio acceder a recursos en otro dominio mientras mantenga los limites de seguridad.  

