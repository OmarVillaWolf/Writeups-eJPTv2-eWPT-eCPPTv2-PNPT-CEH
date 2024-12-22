# Enumeración local de Linux

Tags: #LocalEnumeration #PrivEsc #Linux #Metasploit #Meterpreter

## Enumeración local Automática (Tools) en Linux

```bash 
# Enumeracion con Meterpreter 

❯ sessions <ID>                                    # Regresas a la sesión de Meterpreter que tenias 

❯ background                                       # Colocamos la sesion en segundo plano 
	❯ search enum_configs                         # Enumera las configuraciones de Linux  
	❯ use post/linux/gather/enum_configs           
	❯ options 
	❯ set SESSION <ID>                            # Colocamos el ID de la sesión y lo encontramos con el comando 'sessions'
	❯ run 


❯ background                                       # Colocamos la sesion en segundo plano 
	❯ search enum_network                         # Enumera la informacion acerca de la red   
	❯ use post/linux/gather/enum_network            
	❯ options 
	❯ set SESSION <ID>                            # Colocamos el ID de la sesión y lo encontramos con el comando 'sessions'
	❯ run 


❯ background                                       # Colocamos la sesion en segundo plano 
	❯ search enum_system                          # Muestra informacion del sistema y usuarios Linux  
	❯ use post/linux/gather/enum_system           
	❯ options 
	❯ set SESSION <ID>                            # Colocamos el ID de la sesión y lo encontramos con el comando 'sessions'
	❯ run 


❯ background                                       # Colocamos la sesion en segundo plano 
	❯ search checkvm                              # Detecta si esta corriendo una VM en Linux  
	❯ use post/linux/gather/checkvm           
	❯ options 
	❯ set SESSION <ID>                            # Colocamos el ID de la sesión y lo encontramos con el comando 'sessions'
	❯ run 
```

* [LinEnum](https://github.com/rebootuser/LinEnum)
```bash 
# Herramienta LinEnum

# Lab de eJPT 
❯ Ctrl + Shift + Alt                 # Abre y cierra una consola para copiar codigo de la maquina real a la maquina del lab

# Creas un archivo llamado 'LinEnum.sh' y le pegas el contenido de RAW de la pagina.


# En Meterpreter 
❯ cd C:\\
❯ mkdir Temp
❯ cd Temp                            # El directorio 'Temp' tiene permisos de lectura y escritura 

❯ upload /root/LinEnum.sh            # Cargas el archivo desde tu maquina de atacante a la victima 
❯ shell                              # Cambias a la Shell de Windows 

# comandos Linux 
❯ chmod +x LinEnum.sh                # Le damos permisos de ejecusion  
❯ ./LinEnum.sh                       # Ejecutamos
```