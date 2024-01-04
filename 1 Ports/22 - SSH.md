# SSH 

Tags: #SSH #Puerto #Comandos 

SSH es un protocolo de administración remota que permite a los usuarios **controlar** y **modificar** sus servidores remotos a través de Internet mediante un mecanismo de **autenticación seguro**. Como una alternativa más segura al protocolo **Telnet**, que transmite información sin cifrar, SSH utiliza **técnicas criptográficas** para garantizar que todas las comunicaciones hacia y desde el servidor remoto estén cifradas.

## Practicar 

```bash 
1. https://hub.docker.com/r/linuxserver/openssh-server
2. https://launchpad.net/ubuntu
```

## Comandos 

```bash
❯ ssh ❮User❯@❮IP❯                                  # Para conectarnos por ssh en el puerto default 22
```

```bash
❯ ssh ❮User❯@❮IP❯ -p 2222                          # Para conectarnos por ssh

	# p = En un puerto especifico
```

```bash
❯ sshpass -p <'PASSWORD'> ssh <USER>@<IP>           # Conectarnos por ssh colocando de una vez la passwd  
```

```bash
❯ ssh -i id_rsa <user>@<IP>                         # Nos conectamos por ssh teniendo un id_rsa con privilegio 600
```

```bash
❯ ssh <user>@localhost                             # Cuando tenemos un authorized_key podemos entrar por SSH sin proporcionar passwd en forma local 
```

Cuando estamos en una **restricted bash** (rbash) podemos meter comandos en el ssh, esto si la restricted bash esta mal configurada:
```bash
❯ ssh <User>@<IP> <Command>                        # Podemos ejecuta un comando, y no nos cargara la restricted bash, y en este caso podemos hacer que nos de una bash. 
	bash                                          # Colocamos bash como comando, podremos interactuar aunque no nos de una pseudo-consola. Pero podemos hacer un tratamiento de la consola Linux
```

```bash 
❯ ssh-keygen                                                     # Creamos una clave publica y una clave privada en nuestra maquina de atacante 
❯ cat ~/.ssh/id_rsa.pub | tr -d '\n' | xclip -sel clip           # Miramos el contenido de nuestra clave publica    
	# ~ = /home/omar/...
	# tr = Quitar el salto de linea
	# xclip = Copiarno el output en la clipboard

# El resultado lo pegaremos en el archivo que crearemos con nombre 'authorized_keys' en la ruta de la maquina victima que es /root/.ssh
❯ nvim authorized_keys
```

Descargando el Script, podemos enumerar usuarios 
```bash
❯ searchsploit ssh user enumeration                # Es un exploit en Python2 que lo podemos enocntrar con SearchSploit y debe ser <7.7 de version para que funcione
❯ python2 45939.py <IP> <USER> 2/dev/null          # Para confirmar si ese usuario existe en esa IP de la victima
```


