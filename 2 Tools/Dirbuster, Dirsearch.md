# Dirbuster & Dirsearch 

Tags: #Dirbuster #Escaneo #Fuzzing #Dirsearch


## Dirbuster gráfico 

Esta herramienta sirve para enumerar directorios 
```python
❯ dirbuster &                                   # Ejecutamos el programa y nos saldra una interface como la siguiente:
```

[![Dirbuster.png](https://i.postimg.cc/XN1zQ39m/Dirbuster.png)](https://postimg.cc/LhfD8cC3)

```bash 
1. Ahí colocamos la url empezando por 'http://' y debemos de seguir la sintaxis como la de la imagen
2. Podemos activar la casilla de 'Go Faster' para que el escaneo sea mas rápido 
3. File extension: Para colocar las diferentes extensiones que queremos encontrar, a mayor extensiones, tardara mas 'pdf,docx,rar,php,zip,txt,etc...'
4. Browse: Podemos buscar el diccionario de fuerza bruta '/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt'
```

## Dirb 

```bash 
❯ man dirb                      # Mirar las opciones del comando 'dirb'
```

```python
❯ dirb http://<IP>/             # Enumerar directorios en la web por el puerto 80 

❯ dirb https://<IP>/            # Enumerar directorios en la web por el puerto 443
```

```python
❯ dirb http://<IP> /usr/share/seclists/Discovery/Web-Content/Directory-list-2.3-medium.txt -X .php # Buscar archivos 

	# X = Buscar por una extencion especifica
	# Ruta del diccionario 
	# IP = Maquina a fuzear

# Enumeracion de directorios con un diccionario especifico 
❯ dirb http://<IP> /usr/share/metasploit-framework/data/wordlists/directory.txt 
```

## Dirsearch 

```bash 
❯ dirsearch -u http://IP/ -t 30 -e txt,html,php -f -w /usr/share/seclists/Discovery/Web-Content/Directory-list-2.3-medium.txt

	# u = URL 
	# t = Numero de peticiones 
	# e = Extensiones a buscar 'txt,html,php,jsp,asp,aspx,rar,zip'
	# f = Forzar la busqueda de extensiones 
	# w = Diccionario a utilizar 
```
