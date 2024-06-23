# Introducción 

Tags: #Powershell 

```bash 
❯ https://github.com/PowerShell/PowerShell           # Descargar Powershell 
```

```bash 
❯ powershell /?                          # Panel de ayuda 

❯ powershell.exe -ExectionPolicy Bypass .\script.ps1             # Permitir la ejecucion de scripts en Powershell 
❯ powershell.exe -ExectionPolicy Unrestricted .\script.ps1

❯ powershell.exe -WindowStyle Hidden .\script.ps1                # Evitas que salga la ventana azul de PS al ejecutar el script 

❯ powershell.exe -Command Get-Process                            # Miramos los procesos 
❯ powershell.exe -Command "& {Get-EventLog -LogName security}"   # Miramos los logs del 'security event'

❯ powershell.exe -EncodeCommand $encodeCommand                   # Ejecutar comandos o scripts en base64

❯ powershell.exe -NoProfile .\script.ps1                           

❯ powershell.exe -Version 2   
```

## Cmdlets

```bash 
❯ Get-Process | Sort-Object -Unique | Select-Object ProcessName, Id  # Despliega el 'ProcessName' de los procesos ordenandolos de forma unica y agrega su ID
❯ Get-Process firefox | Sort-Object -Unique | Format-List Path       # Despliega el 'path' del proceso de 'firefox'

❯ Get-Alias -Definition Get-ChildItem                                # Muestra los 'alias' de ese cmdlet 

❯ Get-WmiObject -class win32_operatingsystem | select -Property *    # Muestra toda la info del sistema operativo
❯ Get-WmiObject -class win32_operatingsystem | select -Property * | Select-Object Version # Muestra la version 
❯ Get-WmiObject -class win32_operatingsystem | fl *                  # Muestra en formato de lista la info del sistema

❯ Get-Service "s*" | Sort-Object Status -Descending                  # Muestra los servicio que empiezan con 's'
```

## Módulos 

```bash 
❯ Get-Module -ListAvailable          # Lista de los modulos disponibles  
❯ Import-Module .\module.psm1        # Importar el modulo 

❯ $Env:PSModulePath                  # Mirar el 'path' de los modulos
```

## Scripts

```bash 
# Descubrir puertos en una IP

$ports=(80,445);$ip="192.168.68.1"; foreach($port in $ports) {try{$socket=New-Object System.Net.Sockets.TcpClient($ip,$port);} catch{}; if ($socket -eq $null) {echo $ip":"$port" - Closed";} else{echo $ip":"$port" - Open"; $socket = $null;}}
```

## Objetos

```bash 
❯ Get-Process -Name "firefox" | kill            # Terminar un proceso o aplicacion 
```