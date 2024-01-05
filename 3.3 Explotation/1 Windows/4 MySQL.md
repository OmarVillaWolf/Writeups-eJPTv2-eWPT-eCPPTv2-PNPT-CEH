# Explotación  

Tags: #Windows #Enumeracion #Escaneo #MySQL 

## MySQL 

```bash 
1. Buscar exploits con 'Searchsploit'
2. Hacer fuerza bruta con 'Hydra, Metasploit' para encontrar usuarios y passwd
3. Conectarse al servicio MySQL con un usuario y passwd valido
4. En esta ruta:
	1. 'C:\wamp\www' encontraremos directorios de la base de datos 'wordpress' y sus archivos 
	2. 'C:\wamp\alias' encontraremos archivos como 'phpmyadmin.conf' para poderlo modificar y que se despliegue en la pagina
		1. Al 'AllowOverride all' le dejamos solo la regla de 'allow from all'
		2. Despues de modificar el 'phpmyadmin.conf' debemos de reiniciar el servicio con 'net stop wampapache, net start wampapache'
		3. En el panel de 'phpmyadmin' podemos agregar un usuario en el 'wordpress' para despues ingresar al 'wp-admin' de wordpress
```