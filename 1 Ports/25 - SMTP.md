# SMTP 

Tags: #SMTP

```bash 
❯ smtp_user_enum -M VRFY -U /usr/share/metasploit-framework/data/wordlist/unix_users.txt -t ❮IP❯ # Ataque de Fuerza bruta para identificar usuarios 

	# VRFY = Verificar una lista de usuarios 
	# U = Lista de usuarios 
```

```bash 
❯ telnet <IP> 25                     # Conectarnos al servicio SMTP
	❯ EHLO test.microsoft.com       # Mandar un saludo y ver la respuesta del servicio 
	❯ VRFY user                     # Verificar si un usuario existe y poderlos enumerar
	❯ EXPN user                     # Muestra la actual direccion de envio del alias 
	
	❯ MAIL FROM: root               # Primero definimos el remitente del mensaje
	❯ RCPT TO: Omar                 # Define el destinatario de un mensaje
```