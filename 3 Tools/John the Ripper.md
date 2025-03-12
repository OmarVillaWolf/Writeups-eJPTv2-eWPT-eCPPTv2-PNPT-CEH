# John the Ripper

Tags: #Hash #BruteForce #JohnTheRipper #Fcrackzip #Crackpkcs12 #PFX #OpenSSL #Hash-Identifier 

Es una herramienta de prueba de penetración que se utiliza para probar la seguridad de las passwd. Esta diseñado para descifrar passwd encriptadas en sistemas y aplicaciones.

- Soporte de múltiples formatos de passwd. Es compatible con una amplia gama de passwd, incluyendo Unix, Windows, MD5, SHA y muchos otros.
- Fuerza bruta avanzada: Es capas de realizar ataques de fuerza bruta avanzados, incluyendo ataques de diccionario, ataques basados en patrones y ataques basados en reglas. 
- Modos de operación flexibles: Admite diferentes modos de operación, incluyendo modo de ataque directo, modo de ataque de diccionario y modo de ataque hibrido. 
- Capacidad de paralelización: Es capas de utilizar múltiples núcleos de procesamiento para realizar múltiples intentos de descifrado de passwd simultáneamente, lo que mejora la velocidad y la eficiencia del ataque. 

- Casos de uso: 
	- Pruebas de penetración: Se utiliza para probar la seguridad de sistemas y aplicaciones que utilizan passwd. 
	- Auditoria de passwd: Se utiliza para auditar la fortaleza de la passwd en sistemas y aplicaciones.
	- Recuperación de passwd: Se utiliza para recuperar passwd perdidas u olvidadas. 

```bash
# Tipos de Hashes:

❯ echo -n "2b22337f218b2d82dfc3b6f77e7cb8ec" | wc -c           # Nos muestra el numero de caracteres en una linea

	# MD5 = 32 Caracteres
```

## Identificar Hash

* Pagina para identificar los hashes: [Identificar_Hashes](https://hashes.com/en/tools/hash_identifier) 
* Pagina para crackear los hashes: [Crackear_Hashes](https://crackstation.net/)

```bash
❯ hashid <2b22337f218b2d82dfc3b6f77e7cb8ec> # Podemos saber el tipo de hash, no es muy confiable

❯ hash-identifier                           # Abriremos la tool y desoues colocaremos el hash a encontrar
	2b22337f218b2d82dfc3b6f77e7cb8ec

	# MD5 = Tiene 32 caracteres
```

## ID	Cryptographic Hash Algorithm

| \$P$ | Hash |
|---|---|
| $1 | MD5 |
| $2 o $2a | Blowfish |
| $5 | SHA-256 |
| $6 | SHA-512 |
| \$sha1$ | SHA1crypt |
| \$y$ | Yescrypt |
| \$gy$ | Gost-yescrypt |
| \$7$ | Scrypt |
## John 

```bash 
❯ john --show --format=RAW-MD5 hash.txt          # Mirar la passwd crackeada en un formato especifico 
❯ john --show hash.txt                           # Otra forma de hacerlo 
```

```bash
❯ john --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile>              # Usamos John para crackear un hash con fuerza bruta

	# wordlist = Ruta del diccionario rockyou.txt
	# hashfile = Archivo que contiene el hash a crackear
```

```bash
❯ john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile>    # Crackear un hash con un formato especifico

❯ john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile.txt>

	# format = raw-md5 -> Formato especifico del hash (MD4,MD5, SHA1...)
	# Raw = Tipo de hash estandar (raw-md5, raw-sha1, raw-sha256, whirlpool...)
	# hashfile = Archivo que contiene el hash a crackear
	
# Ejemplo de un hash NT --> 
	administrator:500:LM:NT:::     # Solo necesitaremos la parte de NT para crackear la password
	Administrador:500:42f29043y123fa9c74f23606c6g522b0:71759a1bb2web4da43e676d6b7190711:::
```

```bash
❯ zip2john File.zip > hash   # Devuelva el Hash para crackearlo, el resultado se ingresa en un archivo llamado 'hash'
❯ jhon -w:/usr/share/wordlists/rockyou.txt hash    # Romper el hash obtenido anteriormente
```

```bash 
# Esto se hace cuando tenemos un id_rsa con 'passphrase' o 'Encrypted'
❯ ssh2jhon id_rsa > jhon.txt          # Pasar el id_rsa a hash con la passwd cifrada 
	❯ jhon jhon.txt --wordlist=/usr/share/wordlists/rockyou.txt   # Encontrar la frase 
```

```bash 
❯ keepass2john file.kdbx                 # Para pasar un archivo Keepass a Hash con la passwd cifrada 
	❯ jhon -w:/usr/share/wordlists/rockyou.txt hash # Usando jhon y el diccionario rockyou, romperemos el hash obtenido anteriormente
```

## Fcrackzip

```bash 
❯ fcrackzip -p /usr/share/wordlists/rockyou.txt -b -u File.zip -D # Nos crackea el hash del archivo comprimido '.ZIP' y nos proporciona la passwd 

	# p = Init-passwd-string 
	# b = Bruteforce
	# D = Dicctionary 
	# u = Use-unzip 
```

## Crackpkcs12

```bash 
❯ git clone https://github.com/crackpkcs12/crackpkcs12   # Forma de instalar en Kali
❯ sudo apt install libssl-dev
❯ cd crackpkcs12/
❯ ./configure
❯ make
❯ sudo make install

❯ crackpkcs12 -d /usr/share/wordlists/rockyou.txt certificate.pfx    # Obtener la password del archivo 'PFX' 
	# d = Diccionario 
```

## OpenSSL para PFX

```bash 
❯ openssl pkcs12 -in certificate.pfx -nocerts -out priv-key.pem -nodes # Obtener la 'private-key.pem' del archivo 'PFX'
❯ openssl pkcs12 -in certificate.pfx -nokeys -out certificate.pem      # Obtener el 'certificado.pem' del archivo 'PFX'
```