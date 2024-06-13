# Hashcat

Tags: #HashCat 

```bash
❯ hashcat --help     # Nos muestra el panel de ayuda de la tool y algunos ejemplos

❯ hashcat --example-hashes     # Muestra todos los tipos de hashes 
	❯ hashcat --example-hashes | grep <hash> -B 10 # Filtramos por el hash y leemos 10 lineas arriba del match

# Podemos crakear de uno en uno
❯ hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
❯ hashcat -m 400 -a 0 hash.txt /usr/share/wordlists/rockyou.txt 

	# m = Modo 
		# 0 = MD5
		# 400 = $P$
	# hashes.txt = Archivo que contiene el hash
	# a 0 = Emplear un ataque de fuerza bruta 
```

```bash 
❯ hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt --show

	# show = Muestra las passwd que ya han sido crackeadas 'historial'
```

```bash
❯ hashcat.exe --stdout -r rules/best64.rule hash.txt > passwords  # Podemos hacer y mostrar variantes de la password almacenada en ese archivo hash.txt y nos creamos un diccionario el cual contenga todas esas variantes
❯ hashcat.exe -m 3200 -a 0 hash passwords                         # Con la misma herramienta crackearemos la password pasandole el hash 
```

