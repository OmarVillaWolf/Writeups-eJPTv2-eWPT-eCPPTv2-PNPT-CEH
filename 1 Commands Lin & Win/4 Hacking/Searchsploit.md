# Searchsploit

Tags: #Enumeracion #SearchSploit

* [Exploit-db](https://www.exploit-db.com/)
* [Rapid7](https://www.rapid7.com/db/)

Podemos usar esta herramienta para buscar exploit y ver si alguno nos puede ayudar a la explotación del sistema

```bash
❯ searchsploit -u                             # Actualizar la base de datos 

❯ searchsploit ❮exploit_name❯ -w              # Muestra la url de donde obtiene el exploit

❯ searchsploit -c ❮exploit_name❯              # Podemos escribir un exploit con 'Case Sensitive'

❯ searchsploit ❮exploit_name❯                 # Para buscar un exploit

❯ searchsploit -x ❮exploit_name❯              # Para ver el contenido del exploit 

	# x = examin

❯ searchsploit -m ❮exploit_name❯              # Para descargarnos el exploit .py/.txt 

	# m = move
```
