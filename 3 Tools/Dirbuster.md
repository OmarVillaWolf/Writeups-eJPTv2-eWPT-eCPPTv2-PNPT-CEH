# Dirbuster 

Esta herramienta sirve para enumerar directorios 
```bash
❯ dirbuster&                                   # Ejecutamos el programa y nos saldra una interface como la siguiente:
```

[![Dirbuster](Dirbuster.png)]

* Ahi colocamos la url empezando por **http://** y debemos de seguir la sintaxis como la de la imagen
* Podemos activar la casilla de **Go Faster** para que el escaneo sea mas rapido 
* File extension: Para colocar las diferentes extensiones que queremos encontrar, a mayor extensiones, tardara mas (**pdf,docx,rar,php,zip,txt,etc**)
* Browse: Podemos buscar el diccionario de fuerza bruta (**/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt**)


* Otra forma de usar Dirbuster de manera no grafica
```bash
❯ dirb http://<IP> /usr/share/seclists/Discovery/Web-Content/Directory-list-2.3-medium.txt -X .php

	# X = Buscar por una extencion especifica
	# Ruta del diccionario 
	# IP = Maquina a fuzear
```

