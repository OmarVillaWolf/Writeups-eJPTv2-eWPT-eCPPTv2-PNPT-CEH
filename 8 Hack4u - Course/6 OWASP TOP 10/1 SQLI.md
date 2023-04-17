# OWASP TOP 10

## SQLI

Tags: #SQLI #OWASP #Explotacion 

**SQL Injection** (**SQLI**) es una técnica de ataque utilizada para explotar vulnerabilidades en aplicaciones web que **no validan adecuadamente** la entrada del usuario en la consulta SQL que se envía a la base de datos. Los atacantes pueden utilizar esta técnica para ejecutar consultas SQL maliciosas y obtener información confidencial, como nombres de usuario, contraseñas y otra información almacenada en la base de datos.

Las inyecciones SQL se producen cuando los atacantes insertan código SQL malicioso en los campos de entrada de una aplicación web. Si la aplicación no valida adecuadamente la entrada del usuario, la consulta SQL maliciosa se ejecutará en la base de datos, lo que permitirá al atacante obtener información confidencial o incluso controlar la base de datos.

Hay varios tipos de inyecciones SQL, incluyendo:

-   **Inyección SQL basada en errores**: Este tipo de inyección SQL aprovecha **errores en el código SQL** para obtener información. Por ejemplo, si una consulta devuelve un error con un mensaje específico, se puede utilizar ese mensaje para obtener información adicional del sistema.
-   **Inyección SQL basada en tiempo**: Este tipo de inyección SQL utiliza una consulta que **tarda mucho tiempo en ejecutarse** para obtener información. Por ejemplo, si se utiliza una consulta que realiza una búsqueda en una tabla y se añade un retardo en la consulta, se puede utilizar ese retardo para obtener información adicional
-   **Inyección SQL basada en booleanos**: Este tipo de inyección SQL utiliza consultas con **expresiones booleanas** para obtener información adicional. Por ejemplo, se puede utilizar una consulta con una expresión booleana para determinar si un usuario existe en una base de datos.
-   **Inyección SQL basada en uniones**: Este tipo de inyección SQL utiliza la cláusula “**UNION**” para combinar dos o más consultas en una sola. Por ejemplo, si se utiliza una consulta que devuelve información sobre los usuarios y se agrega una cláusula “**UNION**” con otra consulta que devuelve información sobre los permisos, se puede obtener información adicional sobre los permisos de los usuarios.
-   **Inyección SQL basada en stacked queries**: Este tipo de inyección SQL aprovecha la posibilidad de **ejecutar múltiples consultas** en una sola sentencia para obtener información adicional. Por ejemplo, se puede utilizar una consulta que inserta un registro en una tabla y luego agregar una consulta adicional que devuelve información sobre la tabla.


## Comandos 
