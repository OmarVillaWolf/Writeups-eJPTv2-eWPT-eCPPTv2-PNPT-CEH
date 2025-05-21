# kpasswd

Tags: #Kpasswd #DC 

Este puerto está asociado con keepass, un servicio relacionado con Kerberos utilizado para el cambio de contraseñas.

## Romper un archivo .kdbx

```bash 
❯ keepassxc file.kdbx         # Abrir un archivo Keepass
```

```bash 
❯ keepass2john file.kdbx > hash     # Extraer el hash para creckearlo
❯ john -w:/rocktou.txt hash         # Crackear el hash y obtener la password   
```

## 

```bash 

```