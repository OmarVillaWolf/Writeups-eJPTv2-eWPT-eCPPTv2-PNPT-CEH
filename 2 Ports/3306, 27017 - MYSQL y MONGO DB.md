# MYSQL 

Tags: #MySQL #Servidor #Comandos #Mongo #DB 

### DATABASE - TABLES - COLUMS - DATA 
Asi se conforma una base de datos

```bash
❯ sudo apt install mariadb-server              # Instalar Mariadb
```

```bash
❯ service mariadb start                        # Iniciar el servicio
❯ service mariadb stop                         # Parar el servicio
```


Nos conectamos a la base de datos de esta manera 
```bash 
❯  mysql -u root -p -h <IP>
❯  mysql -u root -p -h <IP>

	# h = Host
	# u = User
	# p= passwd    -> Dar enter, root, admin -> Passwd por defecto
```

```bash
❯  mysql -u ❮User❯ -p                          # Nos conectamos y le proporcionamos la password encontrada en el archivo anterior

	> show databases;                                # Muestra todas las bases de datos existentes
	> use ❮DB_name❯;                                 # Usamos una base de datos especifica
	> show tables;                                   # Mostramos el contenido de las tablas de la base de datos elegida
	> describe ❮Table_name❯;                         # Miramos que columnas existen
	> select count(*) from ❮Table_name❯              # Mirar los registros de la tabla de la DB seleccionada
	> select User,Password from ❮Table_name❯;        # Seleccionamos los campos de una tabla especifica 
	> select * from ❮Table_name❯;                    # Seleccionamos todo de la tabla users
	> select * from ❮Table_name❯ where username=’admin’;
	> create database ❮DB_name❯;                     # Creamos una base de datos
	> drop table ❮Table_name❯;                       # Eliminas la tabla llamada users
	> create table users (id int auto_increment PRIMARY KEY, username varchar(32), password varchar(32), subscription varchar(32));                                  # Creamos una tabla llamada Users y dentro de ella creamos las columnas con sus nombres.
	> insert into users(username, password, subscription) values(“admin”,”admin123”,”no aplica”) # Insertamos en la tabla llamada users los valores para llenarla
	> select load_file("/etc/shadow");        # Si tenemos permisos podemos ver el '/etc/shadow'
```

----
#### Mongo DB

```bash
❯ mongo -u ❮USER❯ -p ❮PASSWD❯ ❮HOST❯ # Nos conectaremos a mongo DB a la parte de scheduler, si nos sale un error como el siguiente: 'Failed global initialization' colocamos el siguiente comando 'export LC_ALL=C'
```

```bash
❯ mongo                              # Con ese comando nos podemos conectar a mongo 
Una vez dentro de mongo hacemos lo siguiente 	
	❯ help                          # Para ver que comando podemos usar y vemos que podemos listar bases de datos 
	❯ show dbs                      # Listamos las bases de datos y encotramos varias como (admin,blog...)
	❯ use blog                      # Usamos la base de datos llamada blog 
	❯ show collections              # Mostramos contenido y encontramos users
	❯ db.users.find()               # Para listar el contenido y encontramos el usuario y password
	❯ bye                           # Salimos de mongo
```

O en lugar de hacer lo de arriba podemos colocar el siguiente comando:
```bash
❯ mongodump                        # Nos crea el directorio dump  
❯ ls                               # Listamos el contenido creado
❯ cd dump/                         # Nos vamos al directorio dump y ahi miramos toda la informacion 
❯ cd blog/                         # Nos metemos al directorio blog y ahi miramos los archivos en formato bson 
❯ bsondump users.bson              # Miramos el usuario y password 
```