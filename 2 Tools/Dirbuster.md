	# Dirbuster 

Tags: #Dirbuster #Escaneo #Fuzzing 


## Escaneo grafico 

Esta herramienta sirve para enumerar directorios 
```python
❯ dirbuster &                                   # Ejecutamos el programa y nos saldra una interface como la siguiente:
```

[![Dirbuster.png](https://i.postimg.cc/XN1zQ39m/Dirbuster.png)](https://postimg.cc/LhfD8cC3)

* Ahí colocamos la url empezando por **http://** y debemos de seguir la sintaxis como la de la imagen
* Podemos activar la casilla de **Go Faster** para que el escaneo sea mas rápido 
* File extension: Para colocar las diferentes extensiones que queremos encontrar, a mayor extensiones, tardara mas (**pdf,docx,rar,php,zip,txt,etc**)
* Browse: Podemos buscar el diccionario de fuerza bruta (**/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt**)


## Escaneo no gráfico 

```python
❯ dirb http://<IP>/             # Para encontrar rutas de directorios en la pagina web  
```

```python
❯ dirb http://<IP> /usr/share/seclists/Discovery/Web-Content/Directory-list-2.3-medium.txt -X .php

	# X = Buscar por una extencion especifica
	# Ruta del diccionario 
	# IP = Maquina a fuzear

# Enumeracion de directorios
❯ dirb http://<IP> /usr/share/metasploit-framework/data/wordlists/directory.txt 
```
