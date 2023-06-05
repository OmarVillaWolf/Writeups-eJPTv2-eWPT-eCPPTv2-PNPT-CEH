## Summary

Tags: 
N
- IP -> 10.10.10.5
- Ports -> TCP (21, 80), UDP (idk)
- OS ->  Windows IIS 7.5
- Services & Applications
    - 21 -> Ftp 
    - 80 -> Microsoft IIS 7.5
 
## Recon
- 21 -> Anonymous login allowed

- /usr/share/davtest/backdoors/**aspx_cmd.aspx**    Nos dara una WebShell al momento de ejecutarse en una pagina Web
- /usr/share/Seclists/Web-Shells/FuzzDB/**nc.exe**    Netcat nos ayuda a conseguir una ReverShell, lo debemos subir a la web

## User
- Comando -> **whoami /priv** Miramos los privilegios de el usuario y encontramos **SeImpersonatePriviledge** (**Enable**) y lo podemos explotar con **Juicy Potatoe** 
- Comando -> **systeminfo** Miramos la informacion del sistema Windows, en donde encontramos la **OS Version 6.1.7600 N/A Build 7600** que es una version muy explotable

Buscamos en **Google** lo siguiente **OS Version 6.1.7600 N/A Build 7600** priviledge escalation y nos sale un (**MS11-046**), el cual volvemos a buscar en **Google** **MS11-046 github exploit** y encontramos el **windows-kernel-exploits** que nos dara un archivo ejecutable llamado **ms11-046.exe** el cual subiremos a la maquina para al momento de ejecutarlo, nos convierta en usuario root (NT Authority System).

## Root
Ejecutamos el **.\\ms11-046.exe** y automaticamente nos convertimos en usuario nt authority\\system.
Solo queda buscar las dos flags user.txt y root.txt en sus respectivos directorios.
C:\\Users\\Babis\\Desktop\\user.txt
C:\\Users\\Administrator\\Desktop\\root.txt 