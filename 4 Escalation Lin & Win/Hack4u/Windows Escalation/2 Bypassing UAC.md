# Bypassing UAC With UACMe

Tags: #Windows #Bypassing #UAC

User Account Control (UAC) es una característica de seguridad de Windows introducida en Windows Vista, usada para prevenir cambios no autorizados desde el sistema operativo. UAC es usado para asegurar que los cambios en el sistema operativo sean aprobados por el admin o cuenta del usuario. 
Los atacantes pueden hacer Bypass UAC para ejecutar archivos ejecutables maliciosos para elevar privilegios. 

[![UAC.png](https://i.postimg.cc/nr0DF9j1/UAC.png)](https://postimg.cc/fSSL5Lv3)

Para que sea exitoso el Bypassing debemos tener acceso a una cuenta que sea parte de un grupo local de administradores.
Existen diferentes herramientas y técnicas para poder hacer le Bypassing, sin embargo, esto dependerá de la versión de Windows que se este ejecutando en el sistema. 

* [UACMe](https://github.com/hfiref0x/UACME)   Esta herramienta funciona desde Windows 7 a Windows 10

```bash 
# HFS es el nombre de la vulnerabilidad que podemos explotar con Metasploit
```