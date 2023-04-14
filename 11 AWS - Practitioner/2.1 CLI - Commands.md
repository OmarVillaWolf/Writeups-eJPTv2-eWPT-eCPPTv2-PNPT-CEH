# Windows CLI ❮❯

Tags: #AWS #Console #CommandAWS

Nota: Los comandos que podamos introducir dependeran de los servicios de cada Usuario

* Podemos configurar el ID:PASSWD en nuestra CLI
```bash
❯ aws configure

	* Nos pedira el AWS Access KEY ID: **AKIA2C7I2RRFQ2ET2OKE**
	* Nos pedira el Secret Access KEY: **XWHpcgdh8dYmaY8acCB1kQVXEco5cMS4ZL8qg3gD**
	* Region 
	* Formato 
```
Con esto ya tendremos una clave de acceso introducida en nuestro CLI de Windows

Los primero dos datos los debemos de sacar de la consola de **AWS** dentro de **IAM > Users > Security Credentials > Access Keys** y ahi creamos la clave de acceso colocanodle una descripcion 


* Este comando nos muestra la lista de todos los usuarios IAM en Json 
```bash
❯ aws iam list-users
```

