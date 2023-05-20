# Intercambio de recursos de origen cruzado (CORS)

Tags: #CORS #OWASP #Explotacion 

El **Intercambio de recursos de origen cruzado** (**CORS**) es un mecanismo que permite que un servidor web **restrinja** el **acceso a recursos** de **diferentes orígenes**, es decir, de diferentes dominios o protocolos. CORS se utiliza para proteger la privacidad y seguridad de los usuarios al evitar que otros sitios web accedan a información confidencial sin permiso.

Supongamos que tenemos una aplicación web en el dominio “**example.com**” que utiliza una API web en el dominio “**api.example.com**” para recuperar datos. Si la aplicación web está correctamente configurada para CORS, solo permitirá solicitudes de origen cruzado desde el dominio “**example.com**” a la API en el dominio “**api.example.com**“. Si se realiza una solicitud desde un dominio diferente, como “**attacker.com**“, la solicitud será bloqueada por el navegador web.

Sin embargo, si la aplicación web no está correctamente configurada para CORS, un atacante podría aprovecharse de esta debilidad para acceder a recursos y datos confidenciales. Por ejemplo, si la aplicación web no valida la autorización del usuario para acceder a los recursos, un atacante podría inyectar código malicioso en una página web para realizar solicitudes a la API de la aplicación en el dominio “**api.example.com**“.

El atacante podría utilizar herramientas automatizadas para probar diferentes valores de encabezados CORS y encontrar una configuración incorrecta que permita la solicitud desde otro dominio. Si el atacante tiene éxito, podría acceder a recursos y datos confidenciales que no deberían estar disponibles desde su sitio web. Por ejemplo, podría recuperar la información de inicio de sesión de los usuarios, modificar los datos de la aplicación, etc.

Para prevenir este tipo de ataque, es importante configurar adecuadamente CORS en la aplicación web y asegurarse de que solo se permitan solicitudes de origen cruzado desde dominios confiables.


## CORS

