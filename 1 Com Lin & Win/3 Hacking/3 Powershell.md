# Powershell

Tags: #Windows #Powershell #Comandos 

```bash 
Extensiones en Powershell:
- .ps1
- .psm1
- .psd1
```

## Variables 

```powershell 
$var = "Hola"         # Definir una variable 
Write-Host $var       # Imprimir la variable 
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