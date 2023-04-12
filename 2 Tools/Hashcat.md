# Hashcat

❯ **hashcat.exe --stdout -r rules/best64.rule hash.txt > passwords**  Podemos hacer y mostrar variantes de la password almacenada en ese archivo hash.txt y nos creamos un diccionario el cual contenga todas esas variantes
❯ **hashcat.exe -m 3200 -a 0 hash passwords** Con la misma herramienta crackearemos la password pasandole el hash 