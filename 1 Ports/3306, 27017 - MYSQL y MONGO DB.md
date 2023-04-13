# MYSQL ❮❯

Tags: #MySQL #Servidor #Comandos #Mongo #DB 

### DATABASE - TABLES - COLUMS - DATA 
Asi se conforma una base de datos

❯ **sudo pat install mariadb-server –** Instalar Mariadb
❯ **service mariadb start –** Iniciar el servicio

Nos conectamos a la base de datos de esta manera 
❯  **mysql -u ❮User❯ -p** Nos conectamos y le proporcionamos la password encontrada en el archivo anterior
	**>show databases;** Muestra las bases de datos
	**>create database *Twitch*;** -Creas una base de datos llamada Twitch
	**>use *mattermost*;** Usamos una base de datos especifica, en este caso llamada mattermost
	**>show tables;** Mostramos el contenido de las tablas de la base de datos
	**>describe *Users*;** Miramos que columnas existen
	**>select *Username,Password* from *Users*;** Seleccionamos los campos de una tabla especifica 
	**>select * from** users; -Seleccionamos todo de la tabla users
	**>select * from** **users** **where** username**=’**admin**’**;
	>drop table *users*;** -Eliminas la tabla llamada users
	**>create table *users* (*id int auto_increment PRIMARY KEY, username varchar(32), password varchar(32), subscription varchar(32)*);** Creamos una tabla llamada Users y dentro de ella creamos las columnas con sus nombres.
	 **>insert into users(*username, password, subscription*) **values(*“admin”,”admin123”,”no aplica”*)** Insertamos en la tabla llamada users los valores para llenarla


----
#### Mongo DB

❯ **mongo -u ❮USER❯ -p ❮PASSWD❯ ❮HOST❯** Nos conectaremos a mongo DB a la parte de scheduler, si nos sale un error como el siguiente: 'Failed global initialization' colocamos el siguiente comando **export LC_ALL=C**

❯ **mongo** Con ese comando nos podemos conectar a mongo 
Una vez dentro de mongo hacemos lo siguiente 	
	**>help** Para ver que comando podemos usar y vemos que podemos listar bases de datos 
	**>show dbs** Listamos las bases de datos y encotramos varias como (admin,blog...)
	**>use blog** Usamos la base de datos llamada blog 
	**>show collections** Mostramos contenido y encontramos users
	**>db.users.find()** Para listar el contenido y encontramos el usuario y password
	**>bye** Salimos de mongo

o en lugar de hacer lo de arriba podemos colocar el siguiente comando:
❯ **mongodump** Nos crea el directorio dump  
❯ **ls** Listamos el contenido creado
❯ **cd dump/** Nos vamos al directorio dump y ahi miramos toda la informacion 
❯ **cd blog/** Nos metemos al directorio blog y ahi miramos los archivos en formato bson 
❯ **bsondump users.bson** Miramos el usuario y password 