## Maquina 2

Tags: #Windows 

- IP -> VMware
- Ports -> TCP (21,80,135,139,445,5985,47001,49152...9), UDP (idk)
- OS ->  Windows 
- Services & Applications
    - 21 -> tcpwrapped
    - 80 -> IIS/8.5

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash 
❯ nmap -sCV -p- <IP>            # Escaneamos todos los puertos y con las otras banderas obtendremos mas informacion 
```

### Puerto 21

```bash
❯ nmap --script ftp-anon -p21 ❮Target IP❯              # ftp-enum = Escanea y mira si el usuario invitado 'Anonymous' esta habilitado

	#  p21 = Indica el puerto que se quiere escanear
	#  Target IP = Dirección IP que se quiere escanear
```



## User


## Root