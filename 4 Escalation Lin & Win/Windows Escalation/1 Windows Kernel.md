# Windows Kernel 

Tags: #Kernel #Windows 

Windows NT es el Kernel que esta precargado en todas las versiones de Windows 

* [Windows-Exploit-Suggester](https://github.com/AonCyberLabs/Windows-Exploit-Suggester)
```bash 
# Descargamos la herramienta anterior en nuestra maquina de atacante
# En la maquina victima de Windows, debemos de ejecutar el sig. comando y copiar todo el resultado para despues poder usarlo con el exploit
❯ systeminfo                                 # Nos muestra la informacion de Windows

# Una vez obtenida la info, la copiamos y creamos un file en nuestra maquina de atacante
❯ nvim win7.txt                              # Pegamos la informacion obtenida del comando anterior en ese archivo
❯ python2 windows-exploit-suggester.py --update    # Actualizar la base de datos 
❯ python2 windows-exploit-suggester.py --database 2023-07-25-mssb.xls --systeminfo ~/Desktop/win7.txt    # Con este comando haremos que con la base de datos mas actual pueda detectar las vulnerabilidades de la maquina victima 
```

* [Windows-Kernel-Exploits](https://github.com/SecWiki/windows-kernel-exploits)
```bash
# Esta pagina en Github es un buscador de exploits para las diferentes versiones de Windows, en la cual podemos descargar los file '.c, .exe'.
```