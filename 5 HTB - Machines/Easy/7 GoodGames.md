## Summary

- IP -> 10.10.11.130
- Ports -> TCP (80), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 80 -> Apache httpd 2.4.51 

## Recon
```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p80 ❮Target IP❯ -oN targeted
```

Utilizamos este comando cuando solo existe el puerto 80 en una maquina.
```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen
```
Pero no reporta nada

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

```bash
❯ curl -s -X GET http://❮IP❯ -I        # Miramos las cabeceras de respuesta de la pagina web 

	# I = i mayuscula
	# s = silence
```
Pero en esta ocasion tampoco enconntramos algo relevante

Ahora buscamos dentro de la web y mirando con Wappalyzer encontramos que esta corriendo como 'Servidor Web' **Flask 2.0.2** y como Lenguaje **Python 3.9.2** Por lo que podriamos decir que se podria acontecer un **SSTI (Server Site Template Injection)**

Despues hacemos Fuzzing, para ver si existen mas directorios.
```bash 
❯ wfuzz -c --hc=404 -hh=9265 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.11.130/FUZZ
```
Y descubrimos algunos directiorios como los siguientes:  (profile, blog, signup, login, logout)
El que nos sirvio es el 'signup' ya que ahi tenemos otra pagina en donde nos podemos registrar.




## User


## Root