# BloodHound 

Tags: #AD #Powershell #Windows #Enumeracion 

Bloodhound es una app en JavaScript la cual puede identificar fácilmente ataques altamente complejo que de otro modo seria imposible identificar ademas puede identificar grupos con permisos excesivos. Los defensores pueden usar Bloohound para identificar y eliminar esas mismas rutas de ataque. 

```bash 
❯ powershell -ep bypass                      # Politica que nos permite ejecutar scripts en Powershell
 	# ep = Ejecutar politicas 


# Enumeracion 
❯ cd .\Bloodhound\Bloodhound\resources\app\collectors    # Ruta donde se encuentra el modulo
❯ . .\SharpHound.ps1                         # Importamos el modulo 
❯ Invoke-Bloodhound -CollectionMethod All    # Enumeramos con el metodo de 'collection' y nos crea un archivo llamado '2024_bloodhound.zip' el cual usaremos en el GUI de la herramienta de Boolhound

# Abrirmos la herramienta de Boodhound colocando la credenciales 'neo4j:Password123' y cargamos el archivo '2024_bloodhound.zip' 

```
