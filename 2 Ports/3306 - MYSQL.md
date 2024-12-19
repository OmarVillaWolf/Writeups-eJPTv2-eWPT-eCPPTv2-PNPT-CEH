 # MYSQL 

Tags: #MySQL #Servidor #Comandos #DB 

### DATABASE - TABLES - COLUMS - DATA 

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
	> select * from ❮Table_name❯;                    # Dumpeas toda la info de la tabla users, incluyendo sus hashes
	> select * from ❮Table_name❯ where username=’admin’;
	> create database ❮DB_name❯;                     # Creamos una base de datos
	> drop table ❮Table_name❯;                       # Eliminas la tabla llamada users
	> create table users (id int auto_increment PRIMARY KEY, username varchar(32), password varchar(32), subscription varchar(32));                                  # Creamos una tabla llamada Users y dentro de ella creamos las columnas con sus nombres.
	> insert into users(username, password, subscription) values(“admin”,”admin123”,”no aplica”) # Insertamos en la tabla llamada users los valores para llenarla
	> select load_file("/etc/shadow");        # Si tenemos permisos podemos ver el '/etc/shadow'
```
