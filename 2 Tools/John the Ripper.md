# John the Ripper

Tags: #Hash 

Es una herramienta de prueba de penetracion que se utiliza para probar la seguridad de las passwd. Esta disenado para desifrar passwd encriptadas en sistemas y aplicaciones.

* **MD5 = 32 Caracteres**


- Soporte de multiples formatos de passwd. Es complatible con una amplia gama de passwd, incluyendo Unix, Windows, MD5, SHA y muchos otros.
- Fuerza bruta avanzada: Es capas de realizar ataques de fuerza bruta avanzados, incluyendo ataques de diccionario, ataques basados en patrones y ataques basados en reglas. 
- Modos de operacion flexibles: Admite diferentes modos de operacion, incluyendo modo de ataque directo, modo de ataque de diccionario y modo de ataque hibrido. 
- Capacidad de paralelizacion: Es capas de utilizar multiples nucleos de procesamiento para realizar multiples intentos de descifrado de passwd simultaneamente, lo que mejora la velocidad y la eficiencia del ataque. 

- Casos de uso: 
	- Pruebas de penetracion: Se utiliza para probar la seguridad de sistemas y aplicaciones que utilizan passwd. 
	- Auditoria de passwd: Se utiliza para auditar la fortaleza de las passwds en sistemas y aplicaciones.
	- Recuperacion de passwds: Se utiliza para recuperar passwds perdidas u olvidadas. 



- John the Ripper is one of the most well known, well-loved and versatile hash cracking tools out there. It combines a fast cracking speed, with an extraordinary range of compatible hash types.

- A hash is a way of taking a piece of data of any length and  representing it in another form that is a fixed length. This masks the original value of the data. This is done by running the original data through a hashing algorithm. There are many popular hashing algorithms, such as MD4,MD5, SHA1 and NTLM.

Wordlist
**/usr/share/wordlists/rockyou.txt**

John 
- Comando -> **john --wordlist=<.path to wordlist> <Hash_file>** Crackear un hash
	- Wordlist -> Ruta del diccionario 
	- Hashfile -> Archivo que contiene el hash

https://hashes.com/en/tools/hash_identifier Pagina para identificar los hashes
**wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py** Herramienta en Python llamada Hash Identifier
	Comando -> **python3 hash-id.py** Para ejecutar el hash identifier


- Comando -> **john --format=[format] --wordlist=<.path to wordlist> <Hash.file>** Crackear un hash con el formato especifico
	- format -> **raw-md5** -> Formato especifico del hash (MD4,MD5, SHA1... ) (raw=tipo de hash estandar) (raw-md5, raw-sha1, raw-sha256, whirlpool...)



