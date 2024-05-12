# Web Enumeration 

Tags: #Web #Enumeracion 

## Enumeración Web

```bash 
Enumeracion Pasiva 
1. Identificar nombres de dominios y titular 
2. Descubrir archivos o directorios escondidos o no permitidos 
3. Identificar la direccion IP del servidor web y su registro de DNS
4. Identificar tecnologias web que estan siendo usadas en el sitio 
5. Deteccion de WAF
6. Identificar subdominios 
7. Identificar la estructura del contenido del sitio web 

Enumeracion Activa 
1. Analizar y descargar el codigo fuente del sitio o aplicacion web 
2. Descubrimiento de puertos y servicios 
3. Fingerprinting al servidor web 
4. Escanear la aplicacion web 
5. Transferencia de zonas DNS
6. Enumeracion de los subdominios con Fuerza Bruta 
```

## Passive Tools 

```bash 
❯ https://whois.domaintools.com/     # Nos regresa detalles y registros del dominio 
❯ whois <Domain name o IP>           # Comando por consola 
```

```bash 
❯ host <Domain name>                 # Nos muestra la IPv4 e IPv6 del dominio ya que es una utilidad DNS
```

```bash 
❯ https://www.netcraft.com/          # Usando la parte de "What's that site running" para detectar tecnologias en la web, informacion y ademas puede detectar si existe un WAF o un Proxy
```

```bash 
❯  # Enumeracion pasiva DNS
```
