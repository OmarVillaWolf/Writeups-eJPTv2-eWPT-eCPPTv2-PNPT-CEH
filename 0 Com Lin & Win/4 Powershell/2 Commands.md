# Powershell Comandos

Tags: #Powershell #Comandos 

## Powershell 

```bash 
❯ powershell /?                          # Panel de ayuda 

❯ cd .\Documents                         # Cambiar de directorio 
	❯ cd C:\\                           # Cambiamos al directorio principal 'C'
	❯ cd PROGRA-1                       # Para entrar al primer directorio sin escribir todo el nombre con espacios
❯ cls                                    # Limpiar la pantalla 
❯ dir                                    # Mostrar el contenido del dir actual 
❯ type user.txt                          # Mirar el contenido de un archivo 
❯ cp .\users.txt C:\Tools\Scripts        # Copiamos un archivo llamado 'users.txt' en la ruta absoluta 'C:\Tools\Scripts'
	cp .\users.txt .\Tools              # Copiamos un archivo a un dir llamado 'Tools'
```

```bash 
❯ net user /domain                        # Mirar el dominio y sus usuarios
```

```bash 
❯ 
❯ Get-AdUser -Filter *                      # Nos muestra el Nombre, Objeto, SID, DistinguishedName (OU) 
❯ Get-AdGroup -Filter *                     # Nos muestra todos los grupos 
❯ Get-AdOrganizationalUnit -Filter *        # Muestra los OU, Politicas de grupo 
```
