# Hashcat

Tags: #HashCat #Hash-Identifier #DictionaryAttack #BruteForce 

## Identificar Hash

* Pagina para identificar los hashes: [Identificar_Hashes](https://hashes.com/en/tools/hash_identifier) 
* Pagina para crackear los hashes: [Crackear_Hashes](https://crackstation.net/)

```bash
❯ hashid <2b22337f218b2d82dfc3b6f77e7cb8ec> # Podemos saber el tipo de hash, no es muy confiable

❯ hash-identifier                           # Abriremos la tool y desoues colocaremos el hash a encontrar
	2b22337f218b2d82dfc3b6f77e7cb8ec

	# MD5 = Tiene 32 caracteres
```

## Ataque de Diccionario 

* [Hashcat-ExampleHashes](https://hashcat.net/wiki/doku.php?id=example_hashes)

```bash
❯ hashcat --help     # Nos muestra el panel de ayuda de la tool y algunos ejemplos

❯ hashcat --example-hashes     # Muestra todos los tipos de hashes 
	❯ hashcat --example-hashes | grep <hash> -B 10 # Filtramos por el hash y leemos 10 lineas arriba del match

# Podemos crakear de uno en uno
❯ hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt -d 1 -D 2
❯ hashcat -m 5600 hashes.txt rockyou.txt -d 1 -D 2
❯ hashcat -m 400 -a 0 hash.txt rockyou.txt -d 1 -D 2
❯ hashcat -m 13100 -a 0 hash.txt rockyou.txt -d 1 -D 2

	# m = Tipo de hash a evaluar (0 = MD5)
	# a = Tipo de ataque  
	# 400 = Inicia con $P$ y es phpass, WordPress (MD5), Joomla (MD5)
	# 5500 = NetNTLMv1 / NetNTLMv1+ESS
	# 5600 = NetNTLMv2 
	# 13100 = Kerberos 5, etype 23, TGS-REP
	# hashes.txt = Archivo que contiene el hash
	# D = Tipo de dispositivo (2 = GPU)
	# d = ID de la GPU a usar en 'OpenCL' (1 = GPU Nvidia con memoria de 8064 MB). Varia en cada maquina 
	# w = Perfil de Workload (1=Low, 2=Default, 3=High, 4=Nightmare). 

❯ hashcat -m 5600 hashes.txt rockyou.txt --force    # Obligar a que las VM ejecuten Hashcat

❯ hashcat -m 5600 hashes.txt rockyou.txt -O         # Aumenta la velocidad del crackeo 
```

```bash 
❯ hashcat -m 0 hashes.txt rockyou.txt --show

	# show = Muestra las passwd que ya han sido crackeadas 'historial'
```

```bash
❯ hashcat.exe --stdout -r rules/best64.rule hash.txt > passwords  # Crear un diccionario con las variantes de la password almacenada en el archivo hash.txt 

❯ hashcat.exe -m 3200 -a 0 hash passwords       # Crackear la password pasandole el 'Hash' 
```

## Fuerza Bruta

```bash 
# Se debe de usar en Windows con la GPU 
❯ hashcat.exe -m 22000 Hash_File -a 3 "?d?d?d?d?d?d?d?d?d?d" -d 1 -D 2 -w 3

	# m = Tipo de hash a evaluar (22000 = WPA-PBKDF2-PMKID+EAPOL )
	# a = Tipo de ataque (3 = Fuerza bruta)
	# "d?" = Cantidad de digitos a probar. A veces funciona sin comillas 
	# D = Tipo de dispositivo (2 = GPU)
	# d = ID de la GPU a usar en 'OpenCL' (1 = GPU Nvidia con memoria de 8064 MB). Varia en cada maquina 
	# w = Perfil de Workload (1=Low, 2=Default, 3=High, 4=Nightmare). 
```