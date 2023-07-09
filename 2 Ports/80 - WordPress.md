# WordPress

Tags: #WordPress #CMS 

WordPress es un sistema de gestión de contenidos. 

## Rutas típicas de WordPress en la web 

```python
/wp-login.php                        # Es la ruta del panel de autenticación 
/wordpress/wp-login.php
/wp-admin.php                        # Es la ruta del panel de autenticación de admin
/wp-json/wp/v2/users/                # Reporta usuarios validos en la pagina de WordPress en formato Json
/wp-content/plugins                  # Para ver si podemos hacer directory listing y ver los plugins existentes


# Rutas en consola 
wordpress.conf                       # Archivo que nos muestra donde esta configurado el WordPress y se encuentra en la ruta /etc/apache2/sites-enabled/ 
wp-config.php                        # Archivo que dispone de credenciales de acceso a la base de datos, se encuentra en la ruta /var/www/html, a veces esta en la ruta /usr/share/wordpress/
```

## WordPress Enumeración 

Formas de **Enumerar**
* **Usuarios** que aparezcan en la pagina Web, podemos validarlos en el panel de autenticación **Admin**

```bash
❯ wpscan --url ❮http://IP/❯                               # Detecta vulnerabilidades en un wordPress
```

```bash
❯ wpscan --url <http://URL/> -e u,vp                                          # Enumeracion 

	# u = Enumerar usuarios
	# vp = Enumerar plugins vulnerables

❯ wpscan --url http://URL/ --passwords /usr/share/wordlists/rockyou.txt       # BruteForce

	# passwords = Enumerar passwords

❯ wpscan --url <http://URL/> -e u,vp --plugins-detection aggressive           # Enumeracion de plugins de manera agresiva 
	
	# plugins-detection = Enumeracion de plugins (mixed, passive, aggressive)
```

El **API Token** lo podemos descargar registrándonos desde la siguiente url:
* [API-Token](https://wpscan.com/register)

```bash
❯ wpscan --url <http://URL/> -e vp --api-token="DFFGB15GD68DG618GD81GRD"     # Enumeracion 

	# vp = Enumerar plugins vulnerables
	# api-token = Nos representa de mejor manera las vulnerabilidades con el API Token
```

```bash
❯ wpscan --url <http://IP/> -U <USER> -P /usr/share/wordlists/rockyou.txt # Fuerza bruta

	# P = Ruta del diccionario 
	# U = Usuario valido
```

```bash
❯ curl -s -X GET "http://IP/" | grep -oP 'plugins/\k[^/]+' | sort -u # Filtramos por plugins en la paggina web y ver si alguno es vulnerable, los podriamos buscar en Searchsploit

	# sort u = Quitar los resultados repetidos y dejar los 'unicos'
	# s = Modo silencioso 
```

**/xmlrpc.php** Si esta expuesto, podemos enumerar credenciales validas y solo acepta peticiones por **POST** y que este estructurada en **XML**
* Debemos de listar los metodos y lo haremos con el codigo del archivo y ver si existe el siguiente **getUsersBlogs** y despues aplicar fuerza bruta
* [Xmlrpc-Abuse](https://nitesculucian.github.io/2019/07/01/exploiting-the-xmlrpc-php-on-all-wordpress-versions/)
```bash
❯ curl -s -X POST "http://IP/xmlrpc.php" -d@file.xml

	# d@ = Indicamos el archivo que usaremos y es que esta abajo 
```

```bash
❯ nano file.xml

	<?xml version="1.0" encoding="utf-8"?> 
	<methodCall> 
	<methodName>system.listMethods</methodName> 
	<params></params> 
	</methodCall>
```
