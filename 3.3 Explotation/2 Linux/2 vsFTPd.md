# Explotación  

Tags: #Linux #Enumeracion #Escaneo #FTP

## vsFTPd

```bash 
1. Ver si se puede usar el usuario anonymous 
2. Buscar en 'Searchsploit' si existen exploits para la version del servicio o con 'Metasploit'
3. Enumerar otros servicios para conseguir usuarios con 'Metasploit'
	1. Hacer ataques de fuerza bruta con 'Hydra' con los usuarios encontrados 
4. Ingresar al servicios FTP con usuarios y passwd validos 
5. Subir una reverse shell, en esta ruta '/usr/share/webshells/' hay diferentes archivos como 'php-reverse-shell.php'
	1. Configuramos el archivo de la reverse shell con nuestra IP y lo subimos por el FTP en la ruta '/var/www/' o en '/var/www/dav'
	2. Ejecutamos el archivo desde la web para obtener la reverse shell
```