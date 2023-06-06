# Conceptos de Hacking 

Tags: #CEH #NIST #LifeCycle #CiberKillChain 

### PDF

![](Apoyo%20CAP%201%20NIST%20800-115.pdf)

![](Apoyo%20CAP%201%20Hacking%20Basic.pdf)

![](APOYO%20CAP%201%20Ataques.pdf)

### Sesion 1

![](Pasted%20image%2020230507173938.png)

* Es una **violacion** a las **politicas**, **metodologias**. 
* **Activo**: Cualquier dispositivo o sistema de informacion que tenga un puerto de red de internet.
* El hacking es el arte. 
* El hacker es el profesional (La persona etica).


![](Pasted%20image%2020230507173924.png)

* El hacker (Factor humano) que es el la persona que estudio, tiene experiencia y se prepara para el red team. 
* Blue Team: Equipo de defensa, son los profesionales que configuran Firewall (politicas), protegen los activos de informacion.
* Red Team: Equipo ofensivo, buscan vulnerabilidades, huecos o brechas de seguridad por medio de herramientas, para violar o romper la seguridad o defensa de blue team. Hacen reportes de como fue la penetracion.
* Purple Team: Equipo hace de los dos anteriores un poco, hacen investigacion permanente, cuando atacan lo hacen por medio de la vulnerabilidades que conocen y ademas siguen investigando mas sobre ellas.


![](Pasted%20image%2020230507173909.png)

Aprender los siguientes conceptos

![](Pasted%20image%2020230507173851.png)

* Cyber Kill Chain: Son 7 pasos, esta metodologia casi no se trabaja pero existe. 
1. Reconocimiento: Hace un reconocimiento de detalles de todos los sistemas de informacion de la empresa (Router, Switch, Gerente, Profesionales de seguridad ofensiva, Profesionales de seguridad defensiva) Si la gente no conoce de seguridad podemos buscar la manera de atacar las DBs, si no escanean las memorias USB por ahi podriamos colocar algo. Un **ataque** es comprometer (tener acceso) un sistema de informacion. Tambien podriamos ver si podemos hacer ataques de DDOs. Es reconocer por donde podriamos hacer el ataque dependiendo de la empresa y los huecos de seguridad (vulnerabilidades).
2. Preparacion: Es donde preparamos el ataque, por ejemplo saber que es lo que vamos a utilizar como unas Tools, un Backdoor, un correo electronico para Phishing.
3. Distribucion: Aqui es donde notificamos como vamos a distribuir el ataque de como lo voy a hacer y para que lo vamos a hacer. Quedo obsoleta. 
4. Explotacion: Es el ataque en vivo (Attack real time), en donde exploto los huecos de seguridad 
5. Instalacion: Es donde instalaremos todo lo que necesitamos, para escalar privilegios, instalar Backdoors, instalar Sniffers para capturar. 
6. Comando y control: Es tener una administracion de lo anterior ya que somos (Root o NT Authority system) 
7. Acciones: Es cuando aprovechamos o sacar lucro de esa parte. Podemos llevarnos las bases de datos. 

![](Pasted%20image%2020230507173835.png)

* Attack Lifecycle: 
Son casi los mismos pasos de lo anterior, pero explicados de una mejor manera.  
* Indicador de compromiso (IoC): Son los nombres, IPs, MAC. 

![](Pasted%20image%2020230507173813.png)

* NIST SP 800-115: Encontraremos alcances, tecnicas, analizis de las tecnicas e identificaciones, validaciones de tecnicas de vulnerabilidades, plan estrategico de seguridad y de administracion. Debemos de saber que es una metodologia (guia) que me ayuda a hacer un pentest o auditoria a un sistema de informacion.
* Esta consta de 4 pasos:
1. Planning: En donde planificamos que vamos a hacer y que vamos a utlizar.
2. Discovery: Vamos a descubrir los servicios, protocolos, IoC (Nombres, IPs, MAC), passwords, servicios expuestos (RDP...), vulnerabilidades. 
3. Attack: Lo hacemos a traves de herramientas (Tools)
4. Reporting: Existen 2 tipos de reportes (Tecnico, Ejecutivo) 
	* Tecnico: Es un informa en donde nosotros especificamos todo muy detallado hacia la parte tecnica. Tiempo de duracion de cada tarea con evidencias, tanto de vulnerabilidades como lo que se instalo. 
	* Ejecutivo: Es un informe que entregamos a gerencia que es muy puntual, en donde no somos tecnicos pero si explicamos las recomendaciones. Manera resumida por asi decirlo. 
* Mitigacion: Es cubrir una vulnerabilidad, cerrar una brecha de seguridad. 

![](Pasted%20image%2020230507173756.png)

Esta parte seria un valor agregado a la metodologia
* Gaining Access: Es acceder al activo de informacion 
* Escalating Privileges: Ser usuario root, administrator. 
**Backoor:** Es una puerta trasera que nos permitira tener acceso al activo por una via o ruta diferente a la original. Teniendo un puerto adicional que nos pueda conectar al servicio. 

![](Pasted%20image%2020230507171934.png)

![](Pasted%20image%2020230507172738.png)

![](Pasted%20image%2020230507173451.png)

![](Pasted%20image%2020230507173625.png)

![](Pasted%20image%2020230507173650.png)


