# Diferentes tipos de Fuzzers 

Tags: #Wfuzz #Gobuster #Ffuf #Fuzzing 

Modo directorio: Listar los directorios y archivos 
	**/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt**
  **/usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt**
Modo DNS: Encontrar los subdominios 
	**/usr/share/Seclists/Discovery/DNS/subdomains-top1million-5000.txt** 
Modo vhost: Encontrar host virtuales que esten configurados en el servidor
	**/usr/share/Seclists/Discovery/DNS/subdomains-top1million-5000.txt** 

# Wfuzz 
```bash
❯ wfuzz -c --hc=404 --hh=12345 -t 200 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H “Host: FUZZ.❮IP❯” https://❮IP❯

	# hc -> HideCode 404
	# c -> Formato colorido
	# hh -> HideCharacters 12345
	# w -> Ruta del diccionario
	# FUZZ -> Donde va a insertar las palabras el diccionario
	# t -> Lanzar tareas en paralelo al mismo tiempo
	# H -> Para enumerar el subdominio, utilizamos esta cabecera 'Host: '
```

```bash
❯ wfuzz -c --hc=403 -t 20 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H “Host: FUZZ.tinder.com” https://tinder.com

	# c -> Permite meter colores
	# t -> Lanzar tareas en paralelo al mismo tiempo
	# w -> Ruta del diccionario
	# FUZZ -> Donde va a insertar las palabras el diccionario
	# H -> Para enumerar el subdominio, utilizamos esta cabecera 'Host: '
	# hc -> HideCode 403
```

```bash
❯ wfuzz -c -L --hc=404,403 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://miwifi.com/FUZZ
❯ wfuzz -c --hc=404,403 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://miwifi.com/FUZZ/

	# L -> Te aplica un Follow Redirect al codigo de estado 301, si no nos muestra nada, le quitamos el **-L** y le colocamos una barra al final 
	# hc -> HideCode 403 y 404
```

```bash
❯ wfuzz -c --sl=216 --hc=404,403 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://miwifi.com/FUZZ

	# sl -> ShowLine es para me muestre ese numero de lineas (l=ele) especifico
```

```bash
❯ wfuzz -c --hl=216 --hc=404,403 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://miwifi.com/FUZZ

	# hl -> HideLine y sirve para ocultar ese numero de lineas
	# hw -> HideWords y sirve para ocultar el numero de palabras
	# sw -> ShowWords y sirve para mostrar el numero de palabras
	# hh -> HideCharacter y sirve para ocultar el numero de caracteres
	# sh -> ShowCharacters y sirve para mostrar el numero de caracteres
```

Conceptos de listas 
```bash
❯ wfuzz -c --hc=404,403 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt https://miwifi.com/FUZZ.html

	# Para enumerar archivos html en una ruta 
```

Creamos un Payload de tipo lista 
```bash
❯ wfuzz -c --hc=404,403 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -z list,html-txt-php https://miwifi.com/FUZZ.FUZ2Z

	# Para enumerar diferentes archivos en una ruta 
	# z -> Crear un payload de tipo lista 
	# FUZ2Z -> Ahi es donde se colocaran las extensiones que hemos definido (html,php,txt)
```

Conceptos de Range 
```bash
❯ wfuzz -c --hw=6515 -t 200 -z range,1-20000 'https://www.mi.com/shop/buy/detail?product_id=FUZZ'

	# z -> Crear un payload de tipo rango
	# hw -> HideWords y sirve para ocultar el numero de palabras
```

Para descubrir si tiene el Plugin de **gwolle-gb** 
```bash
❯ wfuzz -c --hc=404 -t 200 -w wp-plugins.fuzz.txt http://❮IP❯/FUZZ

	# hc -> HideCode 404
	# c -> Formato colorido
	# w -> Ruta del diccionario
	# FUZZ -> Donde va a insertar las palabras el diccionario
	# t -> Lanzar tareas en paralelo al mismo tiempo
```

# Gobuster
Go trabaja muy bien con Sockets y conexiones 
```bash
❯ gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --url https://❮IP❯ -t 200 -k 

	# vhost -> Modo enumeracion VHost Subdominios
	# w -> Ruta del diccionario
	# t -> Lanzar tareas en paralelo al mismo tiempo
	# k -> Para certificados autofirmados para el puerto 443
```

```bash
❯ gobuster vhost -u https://tinder.com -w /usr/share/Seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 20 | grep -v "403"

	# vhost -> Modo enumeracion VHost Subdominios
	# u -> Colocamos la url
	# t -> Lanzar peticiones en paralelo al mismo tiempo
	# w -> Ruta del diccionario
	# grep -v -> Quitamos los que salgan en codigo de estado 403
```

```bash
❯ gobuster dir -u https://miwifi.com -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 200 --add-slash -b 403,404

	# dir -> Modo enumeracion directorios y archivos 
	# u -> Colocamos la url
	# t -> Lanzar peticiones en paralelo al mismo tiempo
	# w -> Ruta del diccionario
	# add-slash -> Ta agrega una barra al final **/** y podremos ver el codigo de estado correspondiente en lugar de 301
	# b -> Para hacer Blacklist a un codigo de estado (403,404) y que no nos lo muestre
```

```bash
❯ gobuster dir -u https://miwifi.com -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 200 -b 403,404 -x php,html,txt

	# dir -> Modo enumeracion directory/file
	# u -> Colocamos la url
	# t -> Lanzar peticiones en paralelo al mismo tiempo
	# w -> Ruta del diccionario
	# b -> Para hacer Blacklist a un codigo de estado (403,404) y que no nos lo muestre
	# x -> Que extensiones queremos buscar (.php,.html,.txt)
```

```bash
❯ gobuster dir -u https://miwifi.com -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 200 -s 200 -x html -b ' '

	# Debemos de colocar ademas de la **s** la **b** pero con una cadena vacia para evitar el error
	# s -> Queremos codigos de estado 200 = OK
	# dir -> Modo enumeracion directory/file
	# u -> Colocamos la url
	# t -> Lanzar peticiones en paralelo al mismo tiempo
	# w -> Ruta del diccionario
```

# Ffuf 
**ffuf** stands for **Fuzz Faster U Fool**. It's a tool used for web enumeration, fuzzing, and directory brute forcing. Y trabaja muy bien con Sockets y Conexiones porque esta programado en Go

```bash
❯ ffuf -h                   # Despliega el panel de ayuda
```

```bash
❯ ffuf -c -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u https://miwifi.com/FUZZ -v 
❯ ffuf -c -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u https://miwifi.com/FUZZ/ 
❯ ffuf -c -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u https://miwifi.com/FUZZ/ --mc=200 

	# c -> Permite meter colores
	# u -> Colocamos la url
	# w -> Ruta del diccionario
	# t -> Lanzar peticiones en paralelo al mismo tiempo
	# v -> verbose, te dice a donde te redirige el codigo 301 y los demas
	# Si queremos hacer el Follow Redirect para el codigo 301 lo mejor es colocarle una **/** al final 
	# mc -> MatchCode y sirve para filtrar por el codigo de estado 200
	# FUZZ -> Ahi se van a sustituir las palabras del diccionario
```

```bash
❯ ffuf -u http://❮IP❯/FUZZ -w /usr/share/seclists/Discovery/Web-Content/big.txt 

	# u -> url 
	# w -> ruta del diccionario 
	# FUZZ -> Ahi se van a sustituir las palabras del diccionario
```


```bash
	# Para encontrar diferentes archivos 
❯ ffuf -u http://❮IP❯/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.tx
	# Encontrar extenciones para las paginas por default 'index'
❯ ffuf -u http://❮IP❯/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt
	# Buscamos por extenciones especificas
❯ ffuf -u http://❮IP❯/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt
	# Para fuzzear los directorios en la pagina web
❯ ffuf -u http://❮IP❯/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt
```


