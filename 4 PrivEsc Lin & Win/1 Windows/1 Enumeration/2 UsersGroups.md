# Enumeración local de Windows 

Tags: #LocalEnumeration #PrivEsc #Windows  #Meterpreter

## Enumeración de usuarios y grupos 

```bash 
# Que estamos buscando?

1. Usuario actual y sus privilegios
2. Información adicional del usuario
3. Otros grupos en el sistema 
4. Grupos
5. Miembros del grupo de administrador 
```

```bash 
# Comandos con Meterpreter 

❯ getprivs                        # Enumeramos los privilegios del usuario actual  

❯ sessions <ID>                   # Regresas a la sesión de Meterpreter que tenias

❯ background                      # Tambien podemos usar (Ctrl + z) para poner la sesión en segundo plano y poder buscar exploits
	❯ search logged_on           # Exploit para enumerar usuarios que estan loggeados en Windows 
	❯ use post/windows/gather/enum_logged_on_users
	❯ options 
	❯ set SESSION <ID>           # Colocamos el ID de la sesión en background y lo podemos obtener con el comando 'sessions'
	❯ run 


❯ shell                           # Generas una consola para comandos Windows 
```

```bash 
# Comandos Windows 

❯ whoami              # Muestra el usuario actual

❯ whoami /priv        # Muestra los privilegios del usuario actual 
	# SetImpersonatePrivilege = En el cual podriamos usar RottenPotato o  JuicyPotato 
	# SetLoadPrivilege = Para usar el recurso de Tarlogic
	# SeLoadDriverPrivilege = Otra utilidad de Tarlogic
	
❯ query user                           # Muestra los usuario actual que esta loggeado, estado, tiempo, ID

❯ net users                            # Muestra todas las otras cuentas en este SO (usuarios)
❯ net user administrator               # Conocer mas detalles de un usuario en particular, grupo local
❯ net localgroup                       # Muestra todos los grupos locales del sistema 
❯ net localgroup administrators        # Muestra información especifica de un grupo local, miembros  
```