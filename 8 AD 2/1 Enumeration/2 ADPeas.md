# ADPeas

Tags: #AD #ADPeas

ADPeas es una herramienta en PowerShell diseñada para enumerar y analizar entornos de Active Directory, facilitando la identificación de configuraciones inseguras, relaciones de confianza, permisos excesivos y posibles vectores de escalación de privilegios. Es ampliamente utilizada en auditorías y pruebas de seguridad por su capacidad para automatizar la recolección de información clave y detectar vulnerabilidades en dominios Windows.

* [ADPeas](https://github.com/61106960/adPEAS)

```powershell 
❯ IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/61106960/adPEAS/main/adPEAS.ps1')

❯ Invoke-adPEAS    # Módulo para hacer la enumeración automatizada
```