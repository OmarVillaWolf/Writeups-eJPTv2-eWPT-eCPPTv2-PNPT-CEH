# Enumeración NFS (Network File System)

Tags: #Hacking #Enumeration #NFS 

## Nmap

```bash 
# Kali Tool 
❯ nmap -p 2049 IP
```

## SuperEnum 

```bash 
# Kali Tool 
❯ ./superenum         # Ejecutamos el programa 
	❯ target.txt     # Colocamos el 'path' del archivo con la IP target 
```

## RPCScan 

```bash 
# Kali Tool 
❯ python3 rpc-scan.py IP --rpc    # Muestra info
```