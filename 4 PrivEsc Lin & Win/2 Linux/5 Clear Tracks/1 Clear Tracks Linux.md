# Escalación de privilegios en Linux

Tags: #Linux #PrivEsc #ClearTracks

## Limpiar huellas en Linux

```bash 
1. Buena practica es almacenar todos los scripts, exploits, binarios en '/tmp' para despues eliminarlos
2. Eliminar el historial de '.bash_history' 
	❯ echo "1 cd /tmp" > .bash_history      # Limpiar el historial manualmente
	❯ history -c                            # Limpiamos todo el historial 
	❯ cat /dev/null > .bash_history         # La mejor manera de quitar todo el contenido en el historial
```