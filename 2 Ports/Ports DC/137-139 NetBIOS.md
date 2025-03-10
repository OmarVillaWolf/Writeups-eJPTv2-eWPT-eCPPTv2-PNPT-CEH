# NetBIOS

Tags: #NetBIOS #DC 

Estos puertos están asociados con el protocolo SMB (Server Message Block) y se utilizan para el acceso a archivos compartidos, impresoras y para la comunicación entre computadoras en una red, funciones comunes en un DC.
## Comandos Windows

```bash 
❯ nbtstat -c                 # Obtener el contenido del cache, tabla de nombres del NetBIOS, IPs
❯ nbtstat -a <IP>            # Obtener el nombre del NtBIOS de la maquina remota    
```

```bash 
❯ net view \\<IP> /ALL       # Muestra los directorios compartidos 
```