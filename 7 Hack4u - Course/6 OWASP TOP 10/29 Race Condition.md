# Condiciones de carrera (Race Condition)

Tags: #RaceCondition #OWASP #Explotacion 

Las **condiciones de carrera** (también conocidas como **Race Condition**) son un tipo de vulnerabilidad que puede ocurrir en sistemas informáticos donde dos o más procesos o hilos de ejecución compiten por los mismos recursos sin que haya un mecanismo adecuado de sincronización para controlar el acceso a los mismos.

Esto significa que si dos procesos intentan acceder a un mismo recurso compartido al mismo tiempo, puede ocurrir que la salida de uno o ambos procesos sea impredecible, o incluso que se produzca un comportamiento no deseado en el sistema.

Los atacantes pueden aprovecharse de las condiciones de carrera para llevar a cabo ataques de denegación de servicio (**DoS**), sobreescribir datos críticos, obtener acceso no autorizado a recursos, o incluso ejecutar código malicioso en el sistema.

Por ejemplo, supongamos que dos procesos intentan acceder a un archivo al mismo tiempo: uno para leer y el otro para escribir. Si no hay un mecanismo adecuado para sincronizar el acceso al archivo, puede ocurrir que el proceso de lectura lea datos incorrectos del archivo, o que el proceso de escritura sobrescriba datos importantes que necesitan ser preservados.

El impacto de las condiciones de carrera en la seguridad depende de la naturaleza del recurso compartido y del tipo de ataque que se pueda llevar a cabo. En general, las condiciones de carrera pueden permitir a los atacantes acceder a recursos críticos, modificar datos importantes, o incluso tomar el control completo del sistema. Por lo tanto, es importante que los desarrolladores y administradores de sistemas tomen medidas para evitar y mitigar las condiciones de carrera en sus sistemas.

A continuación, se os comparte el enlace directo a los proyectos de Github los cuales estaremos empleando en esta clase para practicar las Race Condition:

-   **SKF-LABS Race Condition**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/RaceCondition](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/RaceCondition)
-   **SKF-LABS Race Condition 2**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/RaceCondition-file-write](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/RaceCondition-file-write)


## Race Condition 

* Lab 1: RaceCondition

![](Pasted%20image%2020230524121625.png)

Al momento de ingresar nuestro nombre miramos que se cambia la url. 

![](Pasted%20image%2020230524121713.png)

