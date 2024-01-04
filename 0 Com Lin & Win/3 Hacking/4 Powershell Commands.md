# Powershell Commands

Tags: #Powershell #Comandos 

## Powershell-empire

```bash 
❯ sudo apt install powershell-empire starkiller -y     # Instalamos Poweshell y Starkiller
❯ sudo powershell-empire server                        # Iniciamos el servicio 
❯ sudo powershell-empire client                        # En otra 'terminal' de kali ejecutamos este comando y ahi ejecutaremos los comandos a utilizar
```

```bash 
# Debemos de inciar la app de Starkiller y asi poder usarla. 

	# url = https://localhost:1337
	# u = empireadmin
	# p = password123
```

```bash 
# Todo lo hacemos en el Starkiller

1. Debemos de crear uns 'listener' para podernos conectar al target (http, http_hop)
2. Generar un 'stager' para Windows = 'windows/csharp_exe' y lo descargamos 
3. Transferimos el 'satger' a la maquina victima Windows
4. Ejecutamos el 'stager' como admin en la maquina victima 
5. En la parte de 'Agents' en Starkiller podemos ver la conexion 
	1. En el 'agent' tenemos acceso remoto a la maquina victima 
	2. Podemos ejecutar comandos shell
	3. Podemos ver su arbol de su directorio 'C:\''
	4. Podemos cargar o descargar archivos de sus directorios
```


También podemos usar el Client de Powershell para ejecutar comandos.
```bash 
# Dentro de powershell-empire client, podemos usar los sig comandos

❯ listeners                       # Listar los 'listeners' disponibles 
❯ agents                          # Miramos la conexion que hemos hecho en el Starkiller
❯ interact <agent_name>           # Sirve para ingresar a sistema de la victima 
	❯ help                       # Miramos el menu de ayuda para ver que comandos podemos usar 
	❯ ipconfig 
	❯ history 
```