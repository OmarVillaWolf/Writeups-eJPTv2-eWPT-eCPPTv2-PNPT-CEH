# Ataques de Deserialización

Tags: #Deserializacion #OWASP #Explotacion #Unserialized 

Los **ataques de deserialización** son un tipo de ataque que aprovecha las vulnerabilidades en los procesos de **serialización** y **deserialización** de objetos en aplicaciones que utilizan la programación orientada a objetos (**POO**).

La serialización es el proceso de convertir un objeto en una secuencia de **bytes** que puede ser almacenada o transmitida a través de una red. La deserialización es el proceso inverso, en el que una secuencia de bytes es convertida de nuevo a un objeto. Los ataques de deserialización ocurren cuando un atacante puede manipular los datos que se están deserializando, lo que puede llevar a la **ejecución de código malicioso** en el servidor.

Los ataques de deserialización pueden ocurrir en diferentes tipos de aplicaciones, incluyendo aplicaciones web, aplicaciones móviles y aplicaciones de escritorio. Estos ataques pueden ser explotados de varias maneras, como:

-   Modificar el objeto serializado antes de que sea enviado a la aplicación, lo que puede causar errores en la deserialización y permitir que un atacante ejecute código malicioso.
-   Enviar un objeto serializado malicioso que aproveche una vulnerabilidad en la aplicación para ejecutar código malicioso.
-   Realizar un ataque de “**man-in-the-middle**” para interceptar y modificar el objeto serializado antes de que llegue a la aplicación.

Los ataques de deserialización pueden ser muy peligrosos, ya que pueden permitir que un atacante tome el control completo del servidor o la aplicación que está siendo atacada.

Para evitar estos ataques, es importante que las aplicaciones validen y autentiquen adecuadamente todos los datos que reciben antes de deserializarlos. También es importante utilizar bibliotecas de serialización y deserialización seguras y actualizar regularmente todas las bibliotecas y componentes de la aplicación para corregir posibles vulnerabilidades.

A continuación se os proporciona el enlace directo a la máquina de Vulnhub donde explotamos un ‘**PHP Deserialization Attack**‘:

-   **Máquina Cereal 1**: [https://www.vulnhub.com/entry/cereal-1,703/](https://www.vulnhub.com/entry/cereal-1,703/)

## Comandos 

* PHP Deserialization Attack

Ir descubriendo mas rutas de la web 
```bash
❯ gobuster dir -u https://❮IP❯ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt  -t 20  # Descubrir Directorios

	# w -> Ruta del diccionario
	# t -> Lanzar tareas en paralelo al mismo tiempo
```

Cuando la web es Nginx o Apache y se ve feo, es que esta aplicando **virtualhosting** por lo que debes de agregar el dominio a tu **/etc/hosts**
```bash
❯ gobuster vhosts -u https://❮IP:44441❯ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt  -t 20   # Descubrir Subdominios

	# w -> Ruta del diccionario
	# t -> Lanzar tareas en paralelo al mismo tiempo
```

Empezando con el ataque:
```bash
❯ 
```


* Node Js Deserialization Attack