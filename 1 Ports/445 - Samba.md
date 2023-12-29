# Samba 

Tags: #Samba #Linux #Windows 

Samba es una implementación Linux de SMB, y permite que los sistemas Windows puedan acceder a recursos compartidos y dispositivos de Linux. No es un servicio común, pero lo puedes encontrar en una red interna. Debemos de tener credenciales validas para poder acceder al servicio. 

```bash 
❯ smbmap -H ❮IP❯ -u admin -p <passwd>       # Enumerar los archivos de un usuario especifico
```

```bash 
❯ smbclient -L ❮IP❯ -N                      # Para hacer la consulta con la sesion 'NULL'
❯ smbclient -L ❮IP❯ -U admin                # Nos muesta informacion como: 'Servidor, Workgroup, Sharename'
❯ smbclient //❮IP❯/❮Dir❯ -U admin           # Nos conectaremos a una carpeta con un usuario especifico (Debemos tener la passwd)

	❯ dir                                  # Lista el contenido del dir 
	❯ cd                                   # Cambiamos de directorio
	❯ get <file>                           # Descargar un archivo desde el servidor hacia nuestra maquina
	❯ exit                                 # Nos salimos del servicio Samba
```

```bash 
❯ enum4linux -a ❮IP❯                        # Enumerar toda la informacion del servicio Samba, nos dice si 'null session' esta permitido
❯ enum4linux -a ❮IP❯ -u admin -p <passwd>   # Enumerara todo tipo de informacion del usuario dado 
```