# Nikto

Tags: #Nikto #Escaneo 

Esta herramienta sirve para escanear las vulnerabilidades web. Es bueno si lo usas en CTF como HTB, etc... ya que como es un escaneador de vulnerabilidades web, podría tener problemas al momento de escanear una web y podría ser bloqueado por el firewall si es detectado el escaneo.

```bash
❯ nikto -h http://IP/                           # Asi podemos empezar el escaneo de vulnerabilidades web 
	# h = host 

❯ nikto -h http://IP/index.php?page=arbitrary-file-inclusion.php -Tuning 5 -Display V -o Nikto.html -Format html
	# Tuning 5 = Recuperación remota de archivos: internal web root
	# Display V = Salida detallada
	# o = Output hacia un archivo 
	# Format = Formato del archivo de salida
```