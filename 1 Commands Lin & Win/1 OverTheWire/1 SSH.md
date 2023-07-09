# Conexiones SSH

Tags: #SSH #Linux 

```bash 
❯ ssh < usuario>@< server> -p 22                             # Conexion remota a un servidor por un puerto especifico (p=puerto)
❯ sshpass -p <‘password’> ssh < usuario>@< server> -p 22     # Conexión remota a un servidor por un puerto especifico (1er p=password, y la 2da p=puerto) Con esto entra automaticamente.
❯ export TERM=xterm                                          # Una vez dentro de la consola nueva con esto podemos ocupar el Ctrl + l para hacer clear a la consola.
```
