# Explotación  

Tags: #Windows #Enumeracion #Escaneo #IIS #FTP 

## Microsoft IIS FTP 

```bash 
# IIS puede soportar archivo con la extensión (asp, aspx)

1. Lanzar scripts con Nmap 
2. Ver si se puede usar el usuario anonymous
3. Utilizar Hydra para hacer fuerza bruta (usuarios, passwords)
4. Conectarse al FTP con el usuario y la passwd encontrada 
5. Utilizar Msfvenom para crear una 'reverse shell' y subirla por FTP
	1. Utilizar Metasploit para recibir la conexion 'use multi/handler'
	2. Utilizar el mismo payload que en el Msfvenom
	3. Ejecutar el archivo '.aspx' desde la Web
```