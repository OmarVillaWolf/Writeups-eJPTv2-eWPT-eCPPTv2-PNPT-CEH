# BurpSuite ❮❯

Tags: #BurpSuite #Escaneo  #Web #Enumeracion 

❯ **burpsuite &> /dev/null &** Iniciamos el BurpSuite y lo mandamos a segundo plano
❯ **disown** Para independizar el proceso del BurpSuite

**Target** Nos muestra un mapeo de la app o del sitio visitado
**Proxy** Intercepta cada una de las peticiones o respuestas GET o POST
**Intruder** Permite hacer ataques de fuerza bruta o diccionario
**Repeater** Nos permite enviar varias solicitudes a muestra de respuesta
**Decoder** Nos ayuda a convertir texto plano en alguna codificacion Codificar o Decodificar (Base64, Hex, Bits, URL)
**Comparer** Sirve para comparar dos respuestas. Primero debemos de mandar lo del Proxy al Repeater y las respuestas las pasamos al Comparer

❯ **Click derecho > Copy to file** Para guardar la intercepcion del **Proxy** en un archivo **.req** y asi usarlo con la herramienta **SQL-Map**

❯ **Ctrl + I** Para mandar lo del proxy al intruder (I=i)
❯ **Ctrl + r** Para mandar lo del proxy al repeater
❯ **Ctrl + u** Para url encodear

**/usr/share/wordlists/fasttrack.txt** Diccionario para passwd que generalmente traen los sitios Web 


### Intruder: Sniper

Sirve para realizar ataques de fuerza bruta (o de diccionario). El Sniper nos permite agregar un Payload (una carga util) y probar con un solo parametro (una sola posicion). 
.
	Payload set: 1
	Payload Type: Simple list

Debemos de agregar nuestras propias palabras o cargar una lista.

### Intruder: Battering Ram
Permite tomar tantas posiciones como querramos, pero cada uno de los espacios seran probados con la misma palabra.
.
	Payload set: 1
	Payload Type: Simple list

Aqui podemos cargar el diccionario llamado fasttrack.txt
Lo que hace es que toma la misma palabra y la coloca en todos las posiciones que habilitamos. 

### Intruder: Pitchfork
Permite tener dos posiciones en las cuales podemos tener una lista en cada una o generar un ataque de fuerza bruta cada una.
.
	Payload set: 1
	Payload Type: Simple list

El primer payload se refiere a que ahi podemos colocar una lista de usuarios

.
	Payload set: 2
	Payload Type: Simple list

El segundo payload se refiere a que ahi podemos colocar uns lista que podrian ser de passwd.

Probara las posiciones al mismo tiempo, por lo que el usuario y la passwd correctas se deben de encontrar en la misma posicion, de lo contrario no podria encontrarlo aunque si existan en el diccionario.

### Intruder: Cluster Bomb
Permite escoger dos posiciones (2 payloads).
.
	Payload set: 1
	Payload Type: Simple list

El primer payload se refiere a que ahi podemos colocar una lista de usuarios

.
	Payload set: 2
	Payload Type: Simple list

El segundo payload se refiere a que ahi podemos colocar uns lista que podrian ser de passwd.

En este caso probara primero el primer elemento de la lista 1 y despues probara todos los elementos de la lista 2. Pasara a otro elemento de la lista 1 y despues probara todos los elementos de la lista 2 y asi sucesivamente. 









