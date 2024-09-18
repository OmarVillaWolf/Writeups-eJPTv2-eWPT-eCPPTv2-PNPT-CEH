# Samba 

Tags: #Samba #Linux #Windows 

**Samba**, por otro lado, es una implementación libre y de código abierto del **protocolo SMB**, que se utiliza principalmente en sistemas operativos basados en **Unix** y **Linux**. Samba proporciona una manera de compartir archivos y recursos entre dispositivos de red que ejecutan sistemas operativos diferentes, como Windows y Linux.

## Practicar 

```bash 
1. svn checkout https://github.com/vulhub/vulhub/trunk/samba/CVE-2017-7494
```

## Comandos 

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

## Montura del Samba 

```bash 
# Hacemos la montura para traernos el Samba a nuestra maquina y asi poder mirar mejor las rutas de los direstorios, esto lo podemos hacer en el dir /mnt/ de Kali en nuestra maquina de atacante. Hacer la montura significa que si modificamos algo en nuestra maquina victima, lo hace en la original  

❯ mount -t cifs //IP/dir /mnt/ -o username=null,password=null,domain=,rw # Crear montura sin username,passwd 
	# rw = Crear montura con capacidad de lectura y escritura 
❯ tree                                # Debes estar en el directorio de la montura y lo veras en forma de arbol 
❯ unmount /mnt/                       # Desmontar 
```