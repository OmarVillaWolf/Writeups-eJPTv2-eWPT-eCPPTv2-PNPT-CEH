# Escalación de privilegios en Windows

Tags: #Windows #PrivEsc #ClearTracks

## Limpiar huellas en Windows  

```bash 
1. Buena practica es almacenar todos los scripts, exploits, binarios en 'C:/Temp' para despues eliminarlos
2. Si usamos el modulo de 'persistence', este nos crea un servicio en 'C:\Users\ADMINI~1\AppData\Local\Temp\MnpUdbMa.exe' el cual debemos de eliminar manualmente al finalizar 
	# El momento de usar el exploit 'persistence' nos da una ruta en donde si hacemos lo sig. 
	❯ resource cleanup.rc   # Esto lo hacemos desde Meterpreter y nos limpiara el archivo y servicio que crea 'persistence'
3. Debemos eliminar los logs de los eventos de Windows (opcional) 
	❯ clearev               # Limpiamos todos los logs en Windows y lo hacemos desde Meterpreter  
```