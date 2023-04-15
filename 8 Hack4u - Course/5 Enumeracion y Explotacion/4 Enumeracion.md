## Enumeración del sistema interno

Tags: #Enumeracion 

Esto se hace una vez estemos dentro de la maquina victima.

-   **LSE** (**Linux Smart Enumeration**): Es una herramienta de enumeración para sistemas Linux que permite a los atacantes obtener información detallada sobre la configuración del sistema, los servicios en ejecución y los permisos de archivo. LSE utiliza una variedad de comandos de Linux para recopilar información y presentarla en un formato fácil de entender. Al utilizar LSE, los atacantes pueden detectar posibles vulnerabilidades y encontrar información valiosa para futuros ataques.
-   **Pspy**: Es una herramienta de enumeración de procesos que permite a los atacantes observar los procesos y comandos que se ejecutan en el sistema objetivo a intervalos regulares de tiempo. Pspy es una herramienta útil para la detección de malware y backdoors, así como para la identificación de procesos maliciosos que se ejecutan en segundo plano sin la interacción del usuario.

Asimismo, desarrollaremos un script en Bash ideal para detectar tareas y comandos que se ejecutan en el sistema a intervalos regulares de tiempo, abusando para ello del comando ‘**ps -eo user,command**‘ que nos chivará todo lo que necesitamos.

A continuación, se proporciona el enlace a estas herramientas:

-   **Herramienta LSE**: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
-   **Herramienta PSPY**: [https://github.com/DominicBreuker/pspy](https://github.com/DominicBreuker/pspy)


