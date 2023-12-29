# Comandos básicos Linux

Tags: #Linux #Comandos 


## Directorios 

```bash 
1. BIN - El directorio /bin es un **directorio estático y compartible en el que se almacenan archivos binarios/ejecutables necesarios para el funcionamiento del sistema**. Estos archivos binarios los pueden usar la totalidad de usuarios del sistema operativo.
2. BOOT - Es un directorio estático no compartible que **contiene la totalidad de archivos necesarios para el arranque del ordenador excepto los archivos de configuración**. Algunos de los archivos indispensables para el arranque del sistema que acostumbra a almacenar el directorio /boot son el kernel y el gestor de arranque Grub.
3. DEV - El sistema operativo Gnu-Linux trata los dispositivos de hardware como si fueran un archivo. **Estos archivos que representan nuestros dispositivos de hardware se hallan almacenados en el directorio /dev**.

Algunos de los archivos básicos que podemos encontrar en este directorio son:

- cdrom que representa nuestro dispositivo de CDROM.
- sda que representa nuestro disco duro sata.
- audio que representa nuestra tarjeta de sonido.
- psaux que representa el puerto PS/2.
- lpx que representa nuestra impresora.
- fd0 que representa nuestra disquetera.
4. ETC - El directorio /etc es un directorio estático que contiene los archivos de configuración del sistema operativo. Este directorio también contiene archivos de configuración para controlar el funcionamiento de diversos programas.

Algunos de los archivos de configuración de la carpeta /etc pueden ser sustituidos o complementados por archivos de configuración ubicados en nuestra carpeta personal /home.
5. HOME - El directorio /home se trata de un directorio variable y compartible. Este directorio está **destinado a alojar la totalidad de archivos personales de los distintos usuarios del sistema operativo a excepción del usuario root**. Algunos de los archivos personales almacenados en la carpeta /home son fotografías, documentos de ofimática, vídeos, etc.
6. LIB - El directorio /lib es un **directorio estático y que puede ser compartible**. Este directorio contiene bibliotecas compartidas que son necesarias para arrancar los ejecutables que se almacenan en los directorios /bin y /sbin.
7. MNT - El directorio /mnt tiene la finalidad de albergar los puntos de montaje de los distintos dispositivos de almacenamiento como por ejemplo discos duros externos, particiones de unidades externas, etc.
8. MEDIA - La función del directorio /media es similar a la del directorio /mnt. Este directorio **contiene los puntos de montaje de los medios extraíbles de almacenamiento** como por ejemplo memorias USB, lectores de CD-ROM, unidades de disquete, etc.
9. OPT - El contenido almacenado en el directorio /opt **es estático y compartible**. **La función de este directorio es almacenar programas que no vienen con nuestro sistema operativo** como por ejemplo Spotify, Google-earth, Google Chrome, Teamviewer, etc.
10. PROC - El directorio /proc **se trata de un sistema de archivos virtual**. Este sistema de archivos virtual **nos proporciona información acerca de los distintos procesos y aplicaciones que se están ejecutando en nuestro sistema operativo**.
11. ROOT - El directorio /root se trata de un **directorio variable no compartible**. El directorio /root **es el directorio /home del administrador del sistema** (usuario root).
12. SRV - El directorio /srv se usa para almacenar directorios y datos que usan ciertos servidores que podamos tener instalados en nuestro ordenador.
13. SYS - Directorio que contiene información similar a la del directorio /proc. Dentro de esta carpeta podemos encontrar información estructurada y jerárquica acerca del kernel de nuestro equipo, de nuestras particiones y sistemas de archivo, de nuestros drivers, etc.
14. TMP - El directorio /tmp es donde se crean y se almacenan los archivos temporales y las variables para que los programas puedan funcionar de forma adecuada.
15. USR - El directorio /usr es un directorio compartido y estático. Este directorio es el que contiene la gran mayoría de programas instalados en nuestro sistema operativo.

Todo el contenido almacenado en la carpeta /usr es accesible para todos los usuarios y **su contenido es solo de lectura**.
16. VAR - El directorio /var **contiene archivos de datos variables y temporales como por ejemplo los registros del sistema (logs), los registros de programas que tenemos instalados en el sistema operativo, archivos spool, etc.**

**La principal función del directorio /var es la detectar problemas y solucionarlos**. Se recomienda ubicar el directorio /var en una partición propia, y en caso de no ser posible es recomendable ubicarlo fuera de la partición raíz.
17. SBIN - El directorio /sbin se trata de un **directorio estático y compartible**. Su función es similar al directorio /bin, pero a diferencia del directorio /bin, el directorio /sbin **almacena archivos binarios/ejecutables que solo puede ejecutar el usuario root** o administrador del sistema.
18. LOST-FOUND - Directorio que se crea en las particiones de disco con un sistema de archivos ext después ejecutar herramientas para restaurar y recuperar el sistema operativo como por ejemplo fsch.

Si nuestro sistema no ha presentado problemas este directorio estará completamente vacío. En el caso que hayan habido problemas **este directorio contendrá ficheros y directorios que han sido recuperados tras la caída del sistema operativo**.
```

## Comandos básicos 

```bash 
❯ pwd                    # Miras la ruta en donde te encuentras
❯ which <command>        # Miras la ruta absoluta del comando 

❯ whoami; ls             # Para ejecutar dos comandos al mismos tiempo con el ';'
❯ echo !$                # Muestra el codigo de estado del comando ejecutado
```

```bash 
❯ whoami && ls           # AND = Si el codigo de estado del primer comando es exitoso, se ejecuta el segundo comando
❯ whoami || ls           # OR = Aunque el primer comando no sea exitoso, el segundo comando se ejecutara 
```

```bash
❯ whoam 2>/dev/null      # Redirigimos el error 'STDER' al /dev/null y esto hara que no veamos el output del error
❯ cat /etc/host &>/dev/null   # Rediriges el 'STDER' y el 'STDOUT' para que no muestren los errores
❯ wireshark &>/dev/null       # Abre el programa y no miramos el output
❯ wireshark &>/dev/null &     # Colocamos el programa en segundo plano 
❯ wireshark &>/dev/null & disown    # Colocamos el programa en segundo plano y lo haces independiente 
```

```bash 
❯ touch file.txt              # Creas un archivo 
❯ echo "hola" > file.txt      # Agrega el output del comando dentro del archivo (Si el archivo tiene contenido lo sustituye)
❯ echo "adios" >> file.txt    # Te agrega el output del comando debajo de la ultima linea dentro del archivo 
```

```bash 
# u = usuario, g = grupo, o = otros; r = read, w = write, x = execute 

❯ chmod o+w dir              # Queremos que otros tengan capacidad de escritura en el directorio llamado 'dir'
❯ chmod g-x file.txt         # Quitamos el permiso de ejecusion al grupo para el archivo file.txt

❯ chgrp omar dir             # Le colocas el grupo 'omar' al directorio llamado 'dir'
```

```bash 
❯ useradd omar -s /bin/bash -d /home/omar         # Creas un nuevo usuario
	# s = Asignar una Shell
	# d = Asignas un directorio personal al usuario
❯ passwd omar                                     # Asignas una contraseña al usuario 

❯ chown pepe omar                                 # Colocas de propietario 'pepe' al usuario 'omar'
❯ chgrp pepe omar                                 # Colocas de grupo 'pepe' al usuario 'omar'
❯ chown pepe:root omar                            # Colocas como propietario a 'pepe' y el grupo 'root' al usuario 'omar'

❯ groupadd alumnos                                # Creas un grupo llamado 'alumnos'
❯ usermod -a -G alumnos omar                      # Agregas al grupo 'alumnos' al usuario 'omar'
```

```bash 
❯ chmod +t dir               # Para que otros usuarios no puedan alterar los archivos en el dir, esto pasa por el 'Stiky Bit'
❯ chattr +i -V file.txt      # Chattr = Agregar permisos avanzados a un archivo, i = Inmutable, V = Verbose
❯ lsattr file.txt            # Lsattr = Listar permisos avanzados
```

```bash 
❯ which python | xargs ls -l    # Te ayuda a paralelizar comandos y ejecutar el output del primero mientras ejecuta el segundo comando
```

```bash 
❯ chmod u+s /usr/bin/python3.9                # Asignas el permiso SUID al binario de Python (Aparecera una 's' en los permisos del propietario)
❯ find / -type f -perm -4000 2>/dev/null      # Buscar archivos SUID

❯ chmod g+s /usr/bin/python3.9                # Asignas el permiso SGID al binario de Python (Aparecera una 's' en los permisos del grupo)
❯ find / -perm -2000 2>/dev/null              # SGID esto es a nivel de grupo 'root'
```

```bash 
❯ getcap -r / 2>/dev/null                     # Buscar las capabilities 
	# python3.9 cap_setuid=ep                  Controlar el UID
```

```bash 
❯ hostname -I | awk '{print $1}'              # Awk sirve para delimitar la salida del primer comando, en este caso por el primer argumento 
```

```bash
❯ find / -name passwd 2>/dev/null | xargs ls -l     # Buscar archivos que se llamen asi desde la raiz
❯ find / -group omar -type d 2>/dev/null            # Buscar directorios con un grupo asignado especifico
❯ find / -group omar -type f 2>/dev/null            # Buscar archivos con un grupo asignado especifico
❯ find / -user root -writable 2>/dev/null           # Buscar archivos con capacidad de escritura del usuario root desde la raiz
❯ find / -user root -executable 2>/dev/null         # Buscar archivos con capacidad de ejecusion del usuario root desde la raiz
```