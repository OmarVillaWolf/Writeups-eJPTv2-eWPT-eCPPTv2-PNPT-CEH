# Enumeración de servicio HTTP

Tags: #Web #Reconocimiento #Escaneo  #HTTP #HTTP3 #Metodos

## Practicar 

```bash 
1. svn checkout https://github.com/vulhub/vulhub/trunk/openssl/CVE-2014-0160
```

### Request Headers

1. **GET / POST**: Es el método que nos ayuda a solicitar recursos, en el podemos encontrar el tipo de recurso, protocolo y la versión (HTTP/1.1)
2. **Host**: Es el hostname del servidor (\www.google.com)
3. **Cookie**: Información almacenada del lado del cliente y envía de regreso a el servidor con cada petición
4. **User-Agent**: Esta cabecera lleva la información del usuario, el tipo de navegador, su versión, sistema operativo, tipo y descripción del software por ejemplo: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:36.0) 
5. **Accept**: El tipo de media que acepta el cliente como lo puede ser HTML, XML, JSON e imágenes en la respuesta (text/html, application/xhtml+xml)
6. **Accept-Language**: Son los tipo de lenguaje que acepta
7. **Accept-Encoding**: Los tipos de datos que acepta (gzip, deflate)
8. **Connection**: Es el estado de la conexión (keep-alive)
9. **Authorization:** Credenciales de autenticación, si son requeridas. 

## Métodos

```bash 
1. GET: Se usa para recuperar datos del servidor. Solicita el recurso especificado en la URL y no modifica el estado del servidor. Es un metodo seguro, lo que significa que realizar la misma solicitud GET varias veces no deberia tener ningun efecto secundario.

2. POST: Se utiliza para enviar datos que seran procesado por el servidor. Normalmente incluye dat6os en el cuerpo de la solicitud, y el servidor puede realizar acciones basadas en esos datos. Las solicitudes POST pueden provocar cambios en el estado del servidor.
❯ curl -s -X POST http://❮IP❯/login.php -d "name=omar&password=passwd" -v 
# Podemos mandar data por este método y mirar las cabeceras

3. PUT: Se usa para actualizar o crear un recurso en el servidor en la URL especificada. Reemplaza todo el recurso con la nueva representacion proporcionada en el cuerpo de la solicitud. Si el recurso no existe, PUT puede crearlo.
❯ curl -s -X PUT http://❮IP❯/cmd.txt -d @cmdasp.aspx
	# d = Subir una data que es indicada con el @ y sera el archivo de la cdm
❯ curl http://IP/uploads/ --upload-file file.txt  # Subir un archivo a un directorio especifico 

4. DELETE: Se utiliza para eliminar del servidor el recurso especificado por la URL. Despues de una solicitud de eliminacion ecitosa, el recurso ya no estara disponible en esa URL.
❯ curl -s -X DELETE http://❮IP❯/uploads/file.txt    # Para borrar un archivo en un directorio especifico 

5. PATCH: Se utiliza para aplicar modificaciones parciales a un recurso. Es similar al metodo PUT, pero solo actualiza partes especificas del recurso en lugar de reemplazar todo el recurso.

6. HEAD: Es similar al metodo GET, pero solo recupera los encabezados de respueata y no el cuerpo de la respuesta. A menudo se usa para verificar los encabezados de cosas como la existencia de recursos o las fechas de modificacion.

7. OPTIONS: Se utiliza para recuperar informacion sobre las opciones de comunicacion disponibles para el recurso de destino. Permite a los clientes determinar los metodos y encabezados admitidos para un recurso en particular. 
❯ curl -v -s -X OPTIONS http://❮IP❯/post.php 
# Podemos ver los métodos permitidos en esa página o directorio 

8. MOVE 
❯ curl -s -X MOVE http://❮IP❯/cmd.txt -H "Destination:http://IP/cmd.aspx"
# Si no nos acepta subir la extension anterior, le cambiamos la extension para subir el archivo y en la ruta donde se encuentra lo movemos a la extension del 'aspx'

9. CONNECT

10. TRACE
```

### Response Headers

1. **Código de estado**: 
2. **Server:** Sirve para saber la versión del servidor y si es vulnerable lo podremos explotar.
3. **Set-Cookie:** Usa la cookie que ya teníamos anteriormente y la vuelve a reutilizar para subsecuentes requests.
4. **Content-Type**: Regresa el contenido del tipo de media de la respuesta (text/html, application/Json)
5. **Content-Length**: El tamaño del cuerpo de la respuesta en bytes
6. **Cache-Control:** Directivas del comportamiento 

## Componentes de manejo de sesión

1. **Session Identifier:** Un token único (a menudo un ID sesión) es asignado a cada sesión de usuario. Este token es usado para asociar subsecuentes solicitudes para el usuario con sus datos de sesión.
2. **Session Data:** Información relacionada a la sesión de usuario, como el estatus de autenticacion, preferencias de usuario y data temporal, esta almacenada en el servidor. 
3. **Session Cookies:** Son pequeñas piezas de data almacenadas en el buscador del usuario que contiene el ID de sesión.  Se utilizan para mantener el estado entre el cliente y servidor. 

## Códigos de estado 

* 200 ‘OK’: La solicitud ha tenido éxito.
* 204 ’No content’: La petición se ha completado con éxito, pero su respuesta no tiene
ningún contenido, aunque los encabezados pueden ser útiles.
* 301 ‘Moved Permanently’: Significa que la URI del recurso solicitado ha sido cambiado.
(Redirect)
* 302 ‘Found’: Significa que el recurso de la URI solicitada ha sido cambiado temporalmente.
* 304 ‘Not modified’: Esta es usada para propósitos de caché. Le indica al cliente que la
respuesta no ha sido modificada.
* 400 ‘Bad request’: Significa que el servidor no pudo interpretar la solicitud dada una
sintaxis inválida.
* 401 ‘Unauthorized’: Es necesario autenticar para obtener la respuesta solicitada.
* 403 ‘Forbidden’: El cliente no posee los permisos necesarios para cierto contenido, por lo
que el servidor rechaza la petición.
* 404 Not Found: El servidor no pudo encontrar el contenido solicitado.
* 405 ‘Method not allowed’: El método solicitado es conocido por el servidor pero ha sido
deshabilitado y no puede ser utilizado (Cambiar de GET a POST)
* 500 ‘Internal server error’: El servidor ha encontrado una situación que no sabe cómo
manejarla.
* 501 ‘Not implement’: El método solicitado no está soportado por el servidor y no puede ser
manejado.
* 503 ‘Service unavailable’ : El servidor no está listo para manejar la petición, problemas
comunes es la caída por mantenimiento o está sobrecargado.
* 504 ‘Gateway timeout’ : Esta respuesta de error es dada cuando el servidor está actuando
como una puerta de enlace y no puede obtener una respuesta a tiempo.
* 505 ‘HTTP version not supported’: La versión de HTTP usada en la petición no está
soportada por el servidor.


## Buscar en la Web

```bash 
# Que podemos buscar en una pagina Web
1. Mirar el codigo fuente 'Ctrl + u'
2. Inspeccionar el codigo 'Ctrl + Shift + c'
3. Robots.txt / Sitemap.xml
4. Métodos permitidos 'Curl'
5. Listado de directorios 'Fuzzing o Burpsuite'
6. Nikto
7. Rastreo pasivo 'Burpsuite' crawling
8. SQLMap
9. XSS con XSSer
10. HTTP login con Hydra 'WordPress'
11. Ataque al 'basic auth' con Burpsuite 'Intruder - Sniper' (Cargar diccionario y Payload Processing 'add prefix=admin:' y 'Encode=base64')
12. Fuerza bruta con 'Burpsuite' al 'login Form' con 'Intruder'

# Subdominios 
1. Gobuster, Wfuzz, Ffuz
2. Sublist3r
3. Dnsrecon
4. OSINT (crt.sh)
5. Google Dorks

# Enumeracion de usuarios 
1. Ffuf (Agregando cabeceras y contenido cuando encontramos 'username already exists')
2. Fuerza bruta (Cuando ya tenemos algunos nombres de usuarios)

# Atajos en la Web
Recargar la pagina 'Ctrl + r'
Abrir una pagina en otra pestaña 'Ctrl + Click-Izquierdo'

# Data sensible Expuesta
1. Weak Password Storage 
2. Information Disclosure in Error Messages (Banner)
3. Directory Traversal 
4. Unencrypted Backups
```

## Inyección de comandos en un formulario Web

En Windows, la concatenación de comandos permite ejecutar varios comandos en una sola línea dentro de la línea de comandos o un script batch. Esto es útil para llevar a cabo una secuencia de tareas en un orden determinado sin necesidad de ejecutar cada comando por separado. Hay diferentes operadores disponibles para combinar comandos en Windows.

```bash
1. & = Ejecuta el segundo comando independientemente del resultado del primero.
    
    - Ejemplo: `dir & echo Hola`
        
2. && = Ejecuta el segundo comando solo si el primero se completó con éxito (sin errores).
    
    - Ejemplo: `cd C:\Windows && dir`
        
3. || = Ejecuta el segundo comando solo si el primero falló.
    
    - Ejemplo: `cd C:\NoExiste || echo "La ruta no existe"`
        
4. ; = En algunos shells, como en PowerShell, el punto y coma se utiliza para separar comandos, similar al operador `&` en el CMD.
    
    - Ejemplo (en PowerShell): `Get-ChildItem; Write-Host "Listado completado"`
        
5. () = Los paréntesis se pueden usar para agrupar comandos y controlar el orden de ejecución, especialmente en combinación con los operadores anteriores.
    
    - Ejemplo: `(cd C:\Windows && dir) & echo "Listado completado"`
```

## Usuarios por defecto 

```bash 
# Diferentes tipos de usuario en los SO

En Linux:    www-data, User, Root          
En Windows:  iss apppool\defaultapppool, User, NT Authority\System            
```

## Ruta típica en consola

```python 

/var/www/html/
	config.php                  # Archivo de configuracion de la aplicacion web y puede contener informacion valiosa
	initialize.php              # Archivo que se crea con las instalacion de la web (Similar a wp-config) ya que puede contener credenciales de acceso 'user:passwd' para la DB
```

## Comandos

```bash
❯ http ❮IP❯                                       # Podemos ver las cabeceras 
```

```bash 
❯ browsh --startup-url ❮IP❯                       # Enumeracion del buscador de un Apache en fomra de GUI
```

```bash 
❯ lynx http://❮IP❯                                  # Enumeracion del buscador en fomra de GUI
```

```bash
❯ whatweb ❮http://❮IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

```bash
❯ whatweb ❮http://❮IP❯:❮PORT❯           # Nos dara una breve descripcion del gestor de contenidos por un puerto especifico
```

```bash
❯ whatweb ❮http://❮IP❯ -v

	# v = Miramos las cabeceras de la pagina web, las cuales aveces nos revelan cosas
```

```bash 
❯ wig ❮http://❮IP❯                     # Web Information Gathered, nos reporta las versiones de los servicios en la web
```

```bash
❯ curl ❮IP❯ | bash                     # Lo que hace Curl es obtener un index.html del servidor y despues con el bash haremos que nos interprete la data en bash

❯ curl ❮IP❯ | more       

❯ curl http://IP/cgi-bin/ | more       # Miramos las cabeceras de ese directorio
```

```bash
❯ curl http://❮IP❯ -v                    # Miramos los headers de la pagina web 'Request '

❯ curl -s -X GET http://❮IP❯ -I          # Miramos las cabeceras de respuesta de la pagina web 

	# I = i mayuscula
	# s = silence
```

```bash 
❯ wget "http://❮IP❯/index"               # Obtenemos el archivo index 'descargamos'
```

## HTTP3

Es la próxima y tercera versión principal del Protocolo de Transferencia de Hipertexto utilizado para intercambiar información en la World Wide Web, que sucederá a HTTP/2. HTTP/3 es un borrador basado en un RFC anterior, entonces nombrado "Protocolo de Transferencia de Hipertexto sobre QUIC". 
	Protocolo **UDP**
    [HTTP3-QUIC](https://github.com/cloudflare/quiche)
Para poder usar la herramienta de arriba debemos de instalar lo siguiente:
	[Rush](https://github.com/rust-lang/rustup/issues/686)

```bash
# Ruta para poder ejecutar el http3-client /home/user/quiche/target/debug/examples

❯ ./http3-client ❮https://127.0.0.1❯
```