## Summary

Tags: #Linux 

- IP -> 192.168.68.118
- Ports -> TCP (80, 22), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 ->  OpenSSH 8.2p1 
    - 80 -> Apache httpd 2.4.41

## Launchpad

-   **Launchpad**: [https://launchpad.net/ubuntu](https://launchpad.net/ubuntu)

## Recon

```bash
❯ arp-scan -I ens33 --localnet --ignoredups   # Buscas las IPs en tu red local 
```

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts       # Escaneo en la Capa 4 del modelo OSI

❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

```bash
❯ whatweb ❮http://❮IP❯ -v

	# v = Miramos las cabeceras de la pagina web, las cuales aveces nos revelan cosas
```

```bash 
❯ wfuzz -c --hc=404 --hh=12345 -t 200 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt http://❮IP❯

# Encontramos los siguientes directorios: 'upload, config, js, css'
```

## User


## Root