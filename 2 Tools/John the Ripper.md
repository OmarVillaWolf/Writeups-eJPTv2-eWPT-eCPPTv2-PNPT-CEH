# John the Ripper

Tags: #Hash #BruteForce #John 

Es una herramienta de prueba de penetración que se utiliza para probar la seguridad de las passwd. Esta diseñado para descifrar passwd encriptadas en sistemas y aplicaciones.

- Soporte de múltiples formatos de passwd. Es compatible con una amplia gama de passwd, incluyendo Unix, Windows, MD5, SHA y muchos otros.
- Fuerza bruta avanzada: Es capas de realizar ataques de fuerza bruta avanzados, incluyendo ataques de diccionario, ataques basados en patrones y ataques basados en reglas. 
- Modos de operación flexibles: Admite diferentes modos de operación, incluyendo modo de ataque directo, modo de ataque de diccionario y modo de ataque hibrido. 
- Capacidad de paralelización: Es capas de utilizar múltiples núcleos de procesamiento para realizar múltiples intentos de descifrado de passwd simultáneamente, lo que mejora la velocidad y la eficiencia del ataque. 

- Casos de uso: 
	- Pruebas de penetración: Se utiliza para probar la seguridad de sistemas y aplicaciones que utilizan passwd. 
	- Auditoria de passwd: Se utiliza para auditar la fortaleza de la passwd en sistemas y aplicaciones.
	- Recuperación de passwd: Se utiliza para recuperar passwd perdidas u olvidadas. 

* Tipos de Hashes:
```bash
❯ echo -n "2b22337f218b2d82dfc3b6f77e7cb8ec" | wc -c           # Nos muestra el numero de caracteres en una linea

	# MD5 = 32 Caracteres
```

* Pagina para identificar los hashes: [Identificar_Hashes](https://hashes.com/en/tools/hash_identifier) 
* Pagina para crackear los hashes: [Crackear_Hashes](https://crackstation.net/)

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
❯ john --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile>                              # Usamos John para crackear un hash con fuerza bruta

	# wordlist = Ruta del diccionario rockyou.txt
	# hashfile = Archivo que contiene el hash a crackear
```

```bash
❯ john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt <Hashfile>            # Crackear un hash con un formato especifico

	# format = raw-md5 -> Formato especifico del hash (MD4,MD5, SHA1...)
	# Raw = Tipo de hash estandar (raw-md5, raw-sha1, raw-sha256, whirlpool...)
	# hashfile = Archivo que contiene el hash a crackear
```

```bash
❯ zip2john File.zip > hash                      # Para que nos devuelva el Hash y asi despues poderlo crackear, el resultado lo metemos dentro de un archivo llamado 'hash'

❯ jhon -w:/usr/share/wordlists/rockyou.txt hash # Usando jhon y el diccionario rockyou, romperemos el hash obtenido anteriormente
```

```bash 
❯ fcrackzip -p /usr/share/wordlists/rockyou.txt -b -u File.zip -D # Nos crackea el hash y nos proporciona la passwd 
	# p = Init-passwd-string 
	# b = Bruteforce
	# D = Dicctionary 
	# u = Use-unzip 
```

