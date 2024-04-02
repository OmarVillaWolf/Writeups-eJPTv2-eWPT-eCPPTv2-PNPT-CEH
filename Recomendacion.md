
## Recomendación en el Examen

```bash 
1. Enumerar puerto por puerto 
2. Buscar si las versiones que nos muestra son vulnerables 
3. Enumerar todo en la Web 'Manual (mirar codigo fuente) y con Tools (Dirb, Whatweb)'
	1. Tambien podemos usar 'Nikto' para buscar vulnerabilidades en la web
	2. Podemos usar 'SQLMap' para automatizar la inyeccion
	3. Buscar un 'File upload' para agregar un archivo PHP con una 'Reveshell' hecho con 'MSFVenom'
4. Usar los scripts de Nmap para los diferentes puertos (FTP, SMB, etc...)
5. Usar Metasploit (Auxiliar, Exploit)
6. Usar John the Ripper, Hashcat 
7. SMB 445 (Nmap, Enum4linux, SmbMap, SmbClient, Crackmapexec) - Puerto 135 (RPCClient)
	1. Despues de tener un usuario y passwd validos, nos loggeamos de la siguiente manera:
		1. Consola: psexec 
		2. Metasploit: psexec (hashdump)
			1. Shell (net users)
8. Hydra (FTP, SSH, SMB, MySQL, WordPress)
9. Gestores y sus tools (WordPress, Joomla, Magento, Drupal)
10. MsfVenom (Subir archivos y crear la ReverShell)
11. Pivoting 
```