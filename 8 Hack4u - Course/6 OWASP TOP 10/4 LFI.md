# Local File Inclusion (LFI)

Tags: #LFI #OWASP #Explotacion 

La vulnerabilidad **Local File Inclusion** (**LFI**) es una vulnerabilidad de seguridad informática que se produce cuando una aplicación web **no valida adecuadamente** las entradas de usuario, permitiendo a un atacante **acceder a archivos locales** en el servidor web.

En muchos casos, los atacantes aprovechan la vulnerabilidad de LFI al abusar de parámetros de entrada en la aplicación web. Los parámetros de entrada son datos que los usuarios ingresan en la aplicación web, como las URL o los campos de formulario. Los atacantes pueden manipular los parámetros de entrada para incluir rutas de archivo local en la solicitud, lo que puede permitirles acceder a archivos en el servidor web. Esta técnica se conoce como “**Path Traversal**” y se utiliza comúnmente en ataques de LFI.

En el ataque de Path Traversal, el atacante utiliza caracteres especiales y caracteres de escape en los parámetros de entrada para navegar a través de los directorios del servidor web y acceder a archivos en ubicaciones sensibles del sistema.

Por ejemplo, el atacante podría incluir “**../**” en el parámetro de entrada para navegar hacia arriba en la estructura del directorio y acceder a archivos en ubicaciones sensibles del sistema.

Para prevenir los ataques LFI, es importante que los desarrolladores de aplicaciones web validen y filtren adecuadamente la entrada del usuario, limitando el acceso a los recursos del sistema y asegurándose de que los archivos sólo se puedan incluir desde ubicaciones permitidas. Además, las empresas deben implementar medidas de seguridad adecuadas, como el cifrado de archivos y la limitación del acceso de usuarios no autorizados a los recursos del sistema.

A continuación, se os proporciona el enlace directo a la herramienta que utilizamos al final de esta clase para abusar de los ‘**Filter Chains**‘ y conseguir así ejecución remota de comandos:

-   **PHP Filter Chain Generator**: [https://github.com/synacktiv/php_filter_chain_generator](https://github.com/synacktiv/php_filter_chain_generator)


## Metodos de poder hacer un LFI en los diferentes codigos y sanitizaciones.

Con un LFI podemos no solo cargar archivos que agreguemos la ruta de la pagina, si no tambien cambiar y ver otras rutas en la pagina web.
* **/var/www/html** Ruta de default

Un LFI sin sanitizar
```php
<?php
	$filename = $_GET['filename'];
	include($filename);
?>
```
Para este codigo sin sanitizar podemos hacer lo siguiente en la web y asi cargariamos un archivo:
* **index.php?filename=/etc/passwd**


Con el codigo sanitizado evitamos lo de arriba.
```php
<?php
	$filename = $_GET['filename'];
	include("/var/www/html/" . $filename); 
?>

	// . = Concatenar
	// ruta = Que busque el archivo siempre en esa ruta
```
Si colocamos lo mismo de  **index.php?filename=/etc/passwd** en la ruta final obtendriamos:
**/var/www/html//etc/passwd** -> Por lo que esa ruta no exites y no nos daria nada. 
Pero podriamos hacer un **Directory Path Traversal** de la siguiente manera:
* **/var/www/html/../../../../../etc/passwd** y asi burlar la sanitizacion ya que tendriamos un **/etc/passwd**


Sanitizando de nuevo el codigo evitariamos lo de arriba.
```php
<?php
	$filename = $_GET['filename'];
	$filename = str_replace("../", "", $filename);
	include("/var/www/html/" . $filename); 
?>

	// . = Concatenar
	// ruta = Que busque el archivo siempre en esa ruta
	// str_replace = Donde vea ../ lo convierta a nada 
```
Si hacemos de nuevo un Directory Path Traversal de la manera normal obtendriamos lo siguiente:
**/var/www/html/../../../../../etc/passwd** -> /var/www/htmletc/passwd Y esa ruta no existe
Pero podriaos burlar esa sanitizacion colocandolo de manera doble.
* **/var/www/html/....//....//....//....//....//etc/passwd** y como resultado final tendriamos **/etc/passwd**


q