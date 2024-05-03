## Servicios Windows 

Tags: #WindowsServices

| IIS                              | TCP 80/443   | Servidor web desarrollado por Microsoft que solo corre en Windows                                                                                                                                                      |
| -------------------------------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| WebDAV                           | TCP 80/443   | Extensión HTTP que permite a los clientes update, delete, move and copy files en el servidor web. WebDAV actúa como servidor de archivos.                                                                              |
| SMB/CIFS                         | TCP 445      | Protocolo para compartir archivos entre computadoras o en la red LAN                                                                                                                                                   |
| Samba                            | TCP 445      | Implementación Linux de SMB y es usada para que los sistemas Windows puedan acceder a dispositivos y archivos Linux                                                                                                    |
| NetBIOS                          | TCP 139      |                                                                                                                                                                                                                        |
| RDP                              | TCP 3389     | Protocolo de acceso remoto GUI desarrollado por Microsoft, usado para autenticación remota para Windows                                                                                                                |
| WinRM                            | TCP 5986/443 | Protocolo de manejo remoto en Windows, facilita el acceso remoto con los sistemas Windows                                                                                                                              |
| MySQL/ PostgreSQL/ MSSQL, Oracle | TCP 3306     | El servidor de base de datos almacena y maneja los datos de la aplicación web. Almacena la información del usuario, contenido, configuraciones y otros datos relevantes requeridos para la operación de la aplicación. |

## Servicios Linux  

Tags: #LinuxServices

| Apache Web Server / Ngnix        | TCP 80/443 | Servidor web Apache cubre el 80% de los servidores web globalmente. Son servidores web para HTML                                                                                                                       |
| -------------------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SSH                              | TCP 22     | Protocolo de acceso remoto criptográfico usado para acceder remotamente y controlar sistemas de manera segura                                                                                                          |
| FTP                              | TCP 21     | Protocolo que es usado para facilitar la compartición de archivos entre cliente/servidor y viceversa                                                                                                                   |
| SAMBA                            | TCP 445    | Samba es la implementación de Linux del SMB, y permite a los sistemas Windows acceder a los recursos compartidos y dispositivos Linux                                                                                  |
| MySQL/ PostgreSQL/ MSSQL, Oracle | TCP 3306   | El servidor de base de datos almacena y maneja los datos de la aplicación web. Almacena la información del usuario, contenido, configuraciones y otros datos relevantes requeridos para la operación de la aplicación. |

## Definiciones 

```bash 
1. Amenaza: Cualquier acción malintencionada o situación que ponga en riesgo la seguridad de tus dispositivos y datos en el mundo digital. Las amenazas pueden ser hechas por humanos como: Cibercriminales, hackers, insiders o pueden ser de manera natural como: Inundaciones, terremotos, apagones. 
En la ciberseguirdad las amenazas pueden incluir diferentes tipos de ataques como: Infecciones de malware, phishing, DoS y brechas de seguridad.

2. Riesgo: Son las probabilidades de que una amenaza se materialice y tu información, datos personales o el acceso a tus cuentas bancarias queden expuestas o sean modificadas por delincuentes. 
```

## Amenazas y Riesgos comunes en las Aplicaciones Web 

```bash 
1. XSS: Atacantes injectan scripts maliciosos dentro de las paginas web vistas por otros usuarios, lo que conduce a un acceso no autorizado a datos del usuario, sesiones hijacking y manipulacion del navegador.                                          
2. SQLi: Atacantes manipulan la entrada para injectar codigo SQL malicioso en la database de la aplicacion, lo que lleva a acceso no autorizado de datos, manipulacion de data o comprometer la base de datos.                                             
3. CSRF: Los atacantes enga a los usuarios autenticados para que realicen acciones en una aplicacion web sin saberlo, como cambiar los detalles de la cuenta, explotando sus sesiones activas.                                                           
4. Security Misconfiguration: Los servidores, bases de datos o aplicaciones mal configuradas pueden exponer datos confidenciales o proporcionar puntos de entrada para atacantes.                                                                
5. Sensitive Data Exposure: La falta de proteccion adecuada de los datos confidenciales, como passwords o informacion personal, puede llevar al robo de los datos.                                                                                     
6. Brute-Force and Credentials Stuffing Attacks: Los atacantes usan herramientas automatizadas para adivinar nombres de usuarios y passwords, intentando obtener acceso no autorizado a cuentas de usuario.                                                
7. File Upload Vulnerabilities: Los mecanismos inseguros de carga de archivos pueden permitir a los atacantes cargar archivos ,aliciosos, lo que genera ejecucion de codigo remoto o acceso no autorizado al servidor.                                    
8. DoS, DDoS dirigido: Tienen como objetivo abrumar los servidores de aplicaciones web, provocando interrupciones del servicio y denegando acceso legitimo de los usuarios.                                                                                  
9. SSRF: Los atacantes realizan solicitudes desde el sertvidor a recursos internos o redes externas, potencialmente que provoque el robo de datos o el acceso no autorizado.                                                                                    
10. Inadequate Access Controls: Los controles deacceso debil pueden permitir que usuarios no autorizadfos accedan a funcionalidades restringidas o datos confidenciales.                                                                                   
11. Using components with Known Vulnerabilities: La integracion de componentes de terceros con fallas de seguridas conocidas puede introducir debilidades en la aplicacion web.                                                                             
12. Broken Access Control: Los controles de acceso inadecuados pueden permitir que usuarios no autorizados accedan a funcionalidades restringidas o datos confidenciales.
```

## Componentes de la Aplicación Web

```bash 
1. User Interface (UI): Es la presentacion visual de la aplicacion web vista e interactuada por los usuarios. Incluye elementos como paginas web, formularios, menus, botones y otros componentes.

2. Client-Side Technologies: Estas tecnlogias como (HTML, CSS, JavaScript, Cookies and Local Storage), se utilizan para crear la interfaz de usuario y gestionar las interacciones directamente en el navegador web del usuario.

3. Server-Side Technologies: Estas tecnologias como lenguajes de programacion (PHP, Python, Java, Ruby) se utilizan para implementar la logica de negocio de la aplicacion, procesa las solicitudes de los clientes, accede a las bases de datos y genera contenido dinamico para enviarlo de vuelta a el cliente.

4. Databases: Se utilizan para almacenar y administrar los datos de la aplicacion web. Almacenan informacion del usuario, contenido, configuraciones y otros datos relevantes requeridos para el funcionamiento de la aplicacion. 

5. Aplication Logic: Representa las reglas y procesamiento que rigen el funcionamiento de la aplicacion web. Incluye la autenticacion del usuario, validacion de datos, comprobaciones de seguridad y otras reglas comerciales. 

6. Web Servers: Manejan la solicitus inicial de los clientes y sirven los componentes del lado del cliente, como archivos estaticos (HTML, CSS, JavaScript) a los usuarios.

7. Aplication Servers: Ejecutan el codifo del lado del servidor y manejan el procesamiento dinamico de las solicitudes del cliente. Se comunican con bases de datos, realizan logica empresarial y generan contenido dinamico. 
```

