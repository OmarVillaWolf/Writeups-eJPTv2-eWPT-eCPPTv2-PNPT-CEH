# Enumeración de servicio HTTP/HTTPS 

Tags: #Web #Reconocimiento #Escaneo  #HTTP #HTTPS  #HTTP3 #Metodos

## Practicar 

```bash 
1. svn checkout https://github.com/vulhub/vulhub/trunk/openssl/CVE-2014-0160
```

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

## Cabeceras (Headers)

Cuando realizamos solicitudes al servidor web, el servidor devuelve varios encabezados HTTP. Estos encabezados a veces pueden contener información útil, como el software del servidor web y posiblemente el lenguaje de programación/scripting en uso.

### Request

1. **GET / POST**: Es el método que nos ayuda a solicitar recursos, en el podemos encontrar el tipo
de recurso, protocolo y la versión (HTTP/1.1)
2. **Host**: Es la cabecera del servidor y lleva una IP o un dominio.
3. **Cookie**: Identifica la sesión y su usuario
4. **User-Agent**: Esta cabecera lleva la información del usuario, el tipo de navegador, su
versión, sistema operativo, tipo y descripción del software.
5. **Accept**: El tipo de texto que acepta como lo puede ser HTML, XML, e imágenes.
6. **Accept-Language**: Son los tipo de lenguaje que acepta
7. **Accept-Encoding**: Los tipos de datos que acepta
8. **Connection**: Es el estado de la conexión

### Response 

1. **Código de estado**: 
2. Server: Sirve para saber la versión del servidor y si es vulnerable lo podremos explotar.
3. Set-Cookie: Es la cookie que ya teníamos anteriormente y la volvió a colocar para podernos
autenticar
4. **Content-Type**: Regresa código HTML de código texto, la puedes modificar para que te
acepte otro tipo de texto como Json
5. **Content-Length**: Longitud del contenido

## Métodos

```bash 
1. GET

1. HEAD

1. POST
❯ curl -s -X POST http://IP/login.php -d "name=omar&password=passwd" -v 
# Podemos mandar data por este método y mirar las cabeceras

1. PUT
❯ curl -s -X PUT http://IP/cmd.txt -d @cmdasp.aspx
	# d = Subir una data que es indicada con el @ y sera el archivo de la cdm
❯ curl http://IP/uploads/ --upload-file file.txt  # Subir un archivo a un directorio especifico 

1. MOVE 
❯ curl -s -X MOVE http://IP/cmd.txt -H "Destination:http://IP/cmd.aspx"
# Si no nos acepta subir la extension anterior, le cambiamos la extension para subir el archivo y en la ruta donde se encuentra lo movemos a la extension del 'aspx'

1. DELETE
❯ curl -s -X DELETE http://IP/uploads/file.txt    # Para borrar un archivo en un directorio especifico 

1. CONNECT

1. OPTIONS
❯ curl -s -X OPTIONS http://IP/post.php -v
# Podemos ver los métodos permitidos en esa página o directorio 

1. TRACE
```

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
```

## HTTP: 80

```bash
❯ http ❮IP❯                                       # Podemos ver las cabeceras 
```

```bash 
❯ browsh --startup-url <IP>                       # Enumeracion del buscador de un Apache en fomra de GUI
```

```bash 
❯ lynx http://IP                                  # Enumeracion del buscador en fomra de GUI
```

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

```bash
❯ whatweb ❮http://IP:PORT❯             # Nos dara una breve descripcion del gestor de contenidos por un puerto especifico
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
❯ curl http://IP -v                    # Miramos los headers de la pagina web 

❯ curl -s -X GET http://❮IP❯ -I        # Miramos las cabeceras de respuesta de la pagina web 

	# I = i mayuscula
	# s = silence
```

```bash 
❯ wget "http://IP/index"               # Obtenemos el archivo index 'descargamos'
```

## HTTPS: 443 

Usa el protocolo HTTPS que es más seguro y utiliza el protocolo TLS por debajo.
```bash
❯ openssl s_client -connect ❮dominio.com❯:443   # Para conectarnos al openssl e inspeccionar el certificado

❯ sslyze <dominio.com>                          # Inspeccioanr el certificado SSL
```

```bash
❯ sslscan ❮dominio.com❯:8443                    # Te da informacion del ssl de la maquina y si detecta alguna vulnerabilidad te la representa, podemos colocar el puerto si no es el comun 443
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