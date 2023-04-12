# Comandos Kitty

### Bspwm comandos de la ventana
**Win + Enter** Te muestra una terminal y si lo haces otra vez te despliega otra terminal al lado
**Ctrl + Win + Alt** Preselectores de las flechas
**Ctrl + Win + Alt** **+ Espacio** Quita los preselectores de cada ventana
**Win + s** Convierte a una ventana flotate
**Win + Alt + Flecha**(Arriba, Abajo, Izquierda, Derecha) Resize de la ventana
**Win + f** Full screen
**Win + t** Tamano normal de la ventana
**Win + Click Derecho** Resize de la pantalla con el raton
**Win + q** Sales de la sesion
**Win + Shift + Flechas** Mueve a la ventana que querramos hacia un lado
**Win + Shift + f** Abre Firefox
**Win + Shift + 2** Manda la ventana actual al segundo entorno de trabajo
**Win + Shift + q** Cierra la sesion y te manda a la pantalla de bloqueo
**Win + Shift + r** Recarga los cambios
**Win + 2** Te colocas en el segundo entorno de trabajo
**Win + esc** Aceptar los cambios hechos
**Win + d** Abres Rofi y puedes buscar ahi
**Win + flechas** Cambias de ventana

### Consola Kitty Linux
**apt install kitty -y** Instalamos la terminal Kitty, -y pone automaticamente yes
**Ctrl + Shift + Enter**  Abres multiples ventanas dentro de la misma terminal
**Ctrl + Shift + r**  Modificar el tamano de la kitty, (t=taller, s=samller, q=quit)
**Crtl + Shift + t**  Creas una nueva ventana kitty
**Ctrl + Shift + Alt + t**  Renombras la ventana actual de trabajo
**Ctrl + Shift + .**  Cambias la nueva kitty a la izquiera (.=Moverla hacia la izquierda, ,=moverla a la derecha)
**Crtl + Shift + Flecha(Izquierda)** Te mueves entre las ventanas
**Ctrl + Shift + w** Cierras la ventana de la kitty
**Ctrl + Flecha** Te mueves entre consolas de la kitty
**Ctrl + Shift + z** Te hace el zoom a la ventana
**Ctrl + f** Cambias la ventanas de posicion (f=forwards, b=backwards)
**Ctrl + l** Organizas las diferentes ventanas (l=ele)
**Ctrl + Alt + Raton** Puedes seleccionar mejor
**F1** Copy a (Independiente al comando Ctrl + Shift c) ; Otro copiar adicional
**F2** Paste a (Independiente al comando Ctrl + Shift v)
**F3** Copy b
**F4** Paste b
**Esc dos veces** Pone actomaticamete sudo

### Modo VIM en la zshell
Agregas el modo Vim en la .zshrc
	bindkey -v
	export KEYTIMEOUT=1
	
Activamos el modo VI en la consola de comandos 
**esc** Entras al modo Vim
**i** Modo insert y podremos seguir escribiendo en la consola de comandos

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

### Comandos Nano
**Alt + a** Seleccionas las lineas
**Ctrl + k** Cortar
**Ctrl + u** Pegas
**Ctrl + x** Salir
**Ctrl + o** Guardas

### Comandos de Teclado

**Ctrl + l** Limpias la pantalla sin que se borre lo que estas escribiendo
**Ctrl + Shift + C** Copias
**Ctrl + Shift + V** Pegas
**Ctrl + k** Quitas todo lo que se encuentre a la derecha de donde estas posicionado