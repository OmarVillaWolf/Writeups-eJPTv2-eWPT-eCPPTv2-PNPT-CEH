# Comandos NVIM/NANO ❮❯

Tags: #Shell #Comandos #Editor #NVim #Nano 


NVim Practica Online: [NVim-Online](https://www.openvim.com/)

### Comandos Vim
**i** Modo inserción (ahi puedes empezar a escribir)
**esc** Sales del modo inserción

Todos estos comandos son en modo esc
**Alt + u** Es como si hicieras un **ctrl + z** en windows
**Home** inicio de la linea o **0**
**End** Final de la linea o **$**
**w** Te mueves entre palabras hacia la derecha (una a la vez)
**5** **w** Te mueves en este caso 5 palabras a la derecha (Tu eliges el numero de palabras a desplazar)
**3** **d w** Eliminas tres palabras
**d d** Eliminas toda una linea

**Shift + :**
	**wq** Guardas y sales de Vi (w=write, q=quit)
	**q!** Sales sin guardar

Modo No-Insesion **Filtros**
```bash
❯ **:%s/,/\r/g** Para aplicar una sustitucion 

	# s = Sustitucion 
	# /,/ = Entre / es desde donde inicia (Delimitador)
	# \r = Retorno de carro y lo aplicara para la primer coincidencia 
	# /g = Para aplicar el filtro a todas las coincidencias
```

### Modo VIM en la zshell
Agregas el modo Vim en la .zshrc
	bindkey -v
	export KEYTIMEOUT=1
	
Activamos el modo VI en la consola de comandos 
**esc** Entras al modo Vim
**i** Modo insert y podremos seguir escribiendo en la consola de comandos

### Comandos Nano
**Alt + a** Seleccionas las lineas
**Ctrl + k** Cortar
**Ctrl + u** Pegas
**Ctrl + x** Salir
**Ctrl + s** Guardas