# Credenciales SSH 

Tags: #Linux #SSH 

```bash 
1. Puede que existan las credenciales para SSH en el usuario 
2. Listar el contenido oculto con 'ls -la'
3. Ingresar al dir '.ssh' y buscar el siguiente archivo 'id_rsa' que es la llave privada
4. Mostrar el contenido de la llave privada y copiarlo a la maquina Kali 
5. Crear un archivo llamado 'id_rsa', pegar el contenido y dar permiso 600 
```

```bash
❯ chmod 600 id_rsa                  # Colocar el permiso 600 
❯ ssh -i id_rsa <user>@<IP>         # Conectarse por SSH con un 'id_rsa'
```