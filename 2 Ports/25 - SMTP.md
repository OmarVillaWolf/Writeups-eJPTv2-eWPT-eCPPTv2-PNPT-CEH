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