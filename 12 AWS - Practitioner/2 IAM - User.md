# Usuarios IAM en AWS ❮❯


Tags: #AWS #Console

## Crear un usuario en AWS

IAM: Identity and Access Management es un servicio **Global** en donde no podemos escoger una region especifica.   
* **IAM > Users > Add User** [^1]
El usuario puede pertenecer a diferentes grupos al mismo tiempo.

Ahi le podemos agregar:
* Name
* Especificar si crearemos un **IAM User** o **User en Identity Center**
* Password (Si queremos que cuando inicien sesion ellos la cambien por primera vez)
* Crearle un grupo (Seleccionar un grupo existente)
* Agregarle Tags: Departamento:Inegnieria (Dependiendo del rol de la persona)

Dentro del usuario podemos ver **Permissions, Groups, Tags, Security credentials and Access Advisor**

Podemos asignarle un **Alias** para que poder acceder mas facil al **dominio** de la cuenta **root**, en donde tenemos los usuarios IAM especificos
* **IAM > Dashboard > Account Alias 'Create'**
Y abajo del Account Alias podemos ver un **link** que lo debemos de proprocionar a los usuarios para que pongan su User:Passwd y asi puedan entrar al dominio de su Usuario


## Politicas



## Roles
Podemos asignarlo no solo roles a usuarios, si no tambien a servicios
* **IAM > Roles > Create Role**
Aqui podemos seleccionar las diferentes tipos de confianza como por ejemplo: 
* AWS Service
* AWS Account 
* Web Identity
Podemos seleccionar el primero para estancias **EC2, Lambda** o podemos buscar en la lista el servicio que puede ir enlazado con un rol.
Ahi le podemos agregar:
* Politica de servicios
* Name
* Description default

## Herramientas de seguridad

## Access Advisor
Esta herramienta se encuentra en **IAM > Users > Username > Access Advisor**
* Me da informacion muy concreta de todos aquellos permisos o servicios que el usuario a accedido.



## Referencias

[^1]: Crear un usuario IAM:[AWS-IAM](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/users)

