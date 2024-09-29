# Cryptography

Tags: #CEH 

## Cifrados 

```bash 
Cifrado simetrico      # Utilizas la misma llave para cifrar y descifrar 
Cifrado asimetrico     # Utilizas la llave publica 'A' para cifrar y una llave privada 'B' para descifrar  
```

## Estándar 

```bash 
1. DES 'Data Encryption Standar'         # Cifra y descifra bloques de 64 bits bajo el control de una llave de 56-bits, debido a sus debilidades hoy en dia algunas organizaciones proceden a hacer un triple repetido y se conoce como '3DES' para agregar mas fortaleza 

2. AES 'Advanced Encryption Standar'     # Es un algoritmo de llave simetrica para asegurar material sensible y no-clasificado, es un bloque de cifrado que trabaja la misma operacion multiples veces. Tiene un bloque de 128-bits con llaves de 128, 192 y 256 bits para AES-128, AES-192 y AES-256. 

3. RC4      # Algoritmos simetricos de llave variable orientado a trafico de informacion 
4. RC5      # Algoritmo parametrizado llave variable de 128 bits 
5. RC6      # Es un cifrado de bloque simetrico derivado del RC5 con dos caracteristicas adicionales: 
	* Multiplicacion integra 
	* Uyiliza 4 registros de bit 

6. Twofish      # Utiliza bloques de 128 bits y llaves arriba de 256 bits, es un cifrado Feistel, trabaja rapido con CPU o hardware ademas de ser felxible con apliaciones basadas en red. 

7. Threefish    # Es un bloque de llave simetrica de 256, 512 y 1024. Envulelve tre operaciones 'ARX Addition-Rotation-XOR'. Sus bloques son de 256, 512 y 1024 que envuelven 72, 72 y 80 rounds de computacion.   

8. Serpent      # Usa bloques simetricos de 128-bits con 128,192 o 256-bits de llave. Involucra 32 rounds operativos en 4 bloques de 32-bits usando 8 variables S-boxes con 4-bits por entrada y 4-bits por salida.

9. TEA 'Tiny Encryption Algorithm'   # Es un cifrado Feistel que usa 64 rounds. Hace operaciones con llaves de 128-bits en bloques de 64-bits. Tambien, usa una constante que es definida como 232/the golden ratio 

10. CAST-128 o CAST-5   # Es un bloque de cifrado simetrico el cual usa 12 o 16 'round Feistel network' con 64-bits de bloque. Utiliza una llave variada desde 40 a 128 bits en incrementos de 8-bit. CAST-128 consiste de largo 8x32-bit S-boxes and utiliza llave de mascara (km1) y rotacion de llave (Kr1). La funcion consiste en tres tipos de adicion, sustraccion o XOR. CAST-128 utiliza por defecto cofrado 'GPG y PGP'
```

## Protocolo 

```bash 
1. SSL    # Protocolo de la capa de apliacion para manejar la seguridad de la transmision del mensaje en Internet. Utiliza llave publica asimetrica RSA para cifrar la data transmitida a traves de las conexiones SSL

2. TLS    # Protocolo para establecer una concexion segura entre el cliente y servidor y asegura la privacidad e integridad de la informacion durante la transmision. Utiliza algoritmo RSA con 1024 y 2048-bit.

3. PGP 'Pretty Good Privacy'   # Es un protocolo usado para cifrar y descifrar data que provee autenticacion y privacidad criptografica. A veces es usadi para la compresion, firma digital, cifrado, descifrado de mensajes, emails, archivos y directorios. 
	❯ https://8gwifi.org/pgpencdec.jsp
	❯ https://webencrypt.org/openpgpjs/

4. GPG 'GNU Privacy Guard'     # Es un software de reemplazo de PGP con implementacion gratis al 'OpenPGP standard'. Soporta S/MIME y SSH. Utiliza llaves simetricas y asimetricas para su cifrado. 

5. WOT 'Web of Trust'          # Es un modelo confiable de PGP, OpenPGP y GnuPG system. Es una cadena de una red donde los individuos intermediamente validan los certificados de los demas usando sus firmas ya que todos en la red tienen un certificado de autoridad (CA). 

```

## Tools 

```bash 
1. MD5 y MD6 calculadora 
❯ HashMyFiles
❯ MD5 Calculator 
❯ HashCalc
❯ MD6 Hash Generator
❯ All Hash Generator
❯ md5 hash calculator
❯ Message Digester

# Kali 
❯ md5sum
❯ sha1sum
❯ sha512sum

2. Cifrar texto 
❯ BCTextEncoder
❯ AxCrypt
❯ Microsoft Cryptography Tools
❯ Concealer
❯ SensiGuard
❯ Challenger

3. Toolkits 
❯ OpenSSL
❯ Keyczar
❯ wolfSSL
❯ AES Crypto toolkit
❯ RELIC
❯ PyCrypto

4. Cifrado Email 
❯ RMail
❯ Virtru
❯ ZixMail
❯ Egress Secure Email nad File Transfer
❯ Proofpoint Email Protection 
❯ Paubox

5. Cifrado de disco 
❯ VeraCrypt
❯ Rohos Disk Encryption
❯ BitLocker Windows 
❯ FinalCrypt
❯ Broadcom Encryption
❯ FileVault
❯ Gillsoft Full Disk Encryption
❯ CheckPoint Full Disk Encryption 

6. Cifrado de disco en Linux
❯ Cryptsetup
❯ eCryptfs
❯ Cryptmount
❯ Tomb
❯ CryFS
❯ GnuPG

7. Cifrado de disco en macOS
❯ File Vault 2
❯ VeraCrypt
❯ Sophos SafeGuard Encryption
❯ BestCryp Volume Encryption for Mac
❯ Broadcom Symantec Endpoint Encryption  
❯ Rohos Logon Key for Mac
```