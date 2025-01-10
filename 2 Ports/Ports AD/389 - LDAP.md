# Inyecciones LDAP

Tags: #LDAP #OWASP #Explotacion #AD 

* [LDAP](https://www.profesionalreview.com/2019/01/05/ldap/)

Las inyecciones **LDAP** (**Protocolo de Directorio Ligero**) son un tipo de ataque en el que se aprovechan las vulnerabilidades en las aplicaciones web que interactúan con un servidor LDAP. El servidor LDAP es un directorio que se utiliza para almacenar información de usuarios y recursos en una red.

La inyección LDAP funciona mediante la inserción de comandos LDAP maliciosos en los campos de entrada de una aplicación web, que luego son enviados al servidor LDAP para su procesamiento. Si la aplicación web no está diseñada adecuadamente para manejar la entrada del usuario, un atacante puede aprovechar esta debilidad para realizar operaciones no autorizadas en el servidor LDAP.

Al igual que las inyecciones SQL y NoSQL, las inyecciones LDAP pueden ser muy peligrosas. Algunos ejemplos de lo que un atacante podría lograr mediante una inyección LDAP incluyen:

-   Acceder a información de usuarios o recursos que no debería tener acceso.
-   Realizar cambios no autorizados en la base de datos del servidor LDAP, como agregar o eliminar usuarios o recursos.
-   Realizar operaciones maliciosas en la red, como lanzar ataques de phishing o instalar software malicioso en los sistemas de la red.

Para evitar las inyecciones LDAP, las aplicaciones web que interactúan con un servidor LDAP deben validar y limpiar adecuadamente la entrada del usuario antes de enviarla al servidor LDAP. Esto incluye la validación de la sintaxis de los campos de entrada, la eliminación de caracteres especiales y la limitación de los comandos que pueden ser ejecutados en el servidor LDAP.

También es importante que las aplicaciones web se ejecuten con privilegios mínimos en la red y que se monitoreen regularmente las actividades del servidor LDAP para detectar posibles inyecciones.

A continuación, se proporciona el enlace directo al proyecto de GitHub que nos descargamos para desplegar un laboratorio práctico donde poder ejecutar esta vulnerabilidad:

-   **LDAP-Injection-Vuln-App**: [https://github.com/motikan2010/LDAP-Injection-Vuln-App](https://github.com/motikan2010/LDAP-Injection-Vuln-App)

Este es el puerto estándar para LDAP (Lightweight Directory Access Protocol), que se utiliza para acceder y gestionar los servicios de directorio. En el contexto de AD, LDAP se usa para realizar consultas y cambios en el directorio de AD.

## LDAP

* [LDAP-Pentesting](https://book.hacktricks.xyz/network-services-pentesting/pentesting-ldap )

```bash 
❯ ldapdomaindump -u 'domain\user' -p 'password' IP     # Crea reportes para poder ver la info desde la web
```

```bash 
❯ ldapsearch -x -h <IP> -s base namingcontexts         # Hacemos un 'simple authentication' para enumerar 
❯ ldapsearch -x -h <IP> -b 'DC=example'                # Enumeramos el dominio 
```

```bash
❯ ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin 'cn=admin'   # Podemos ver informacion del usuario admin 

	# b,dc,dc = Base de busqueda, que el dominio seria example.org
	# D = Nombre distinguido
	# 1er_cn = Usuario a autenticar
	# w = Password 
	# x = Autenticacion simple (Usuario, Passwd)
	# 2do_cn = Criterio de busqueda y si existe que nos reporte la informacion  
```

```bash
❯ ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin 'cn=omar'    # Para enumerar un usuario especifico como 'omar'
```

```bash
❯ ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin '(&(cn=omar)(description=LDAP*))'

	# (&(cn=omar)(description=LDAP*)) = Le decimos que el usuario a buscar es omar 'y' su descripcion es LDAP, con * le decimos que tiene mas contenido despues de LDAP
	# (|(cn=omar)(description=LDAP*)) = Le decimos que el usuario a buscar es omar 'o' su descripcion es LDAP, con * le decimos que tiene mas contenido despues de LDAP
```

```bash
❯ ldapadd -x -H ldap://localhost -D "cn=admin,dc=example,dc=org" -w admin -f newuser.ldif  # Creara un usuario en base a un archivo que le pasemos
```
