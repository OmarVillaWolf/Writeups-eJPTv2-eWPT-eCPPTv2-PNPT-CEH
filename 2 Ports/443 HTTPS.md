# Enumeración de servicio HTTPS

Tags: #Web #Reconocimiento #Escaneo  #HTTPS 

## Comandos

```bash
❯ openssl s_client -connect ❮dominio.com❯:443   # Para conectarnos al openssl e inspeccionar el certificado

❯ sslyze <dominio.com>                          # Inspeccioanr el certificado SSL
```

```bash
❯ sslscan ❮dominio.com❯:8443                    # Te da informacion del ssl de la maquina y si detecta alguna vulnerabilidad te la representa, podemos colocar el puerto si no es el comun 443
```