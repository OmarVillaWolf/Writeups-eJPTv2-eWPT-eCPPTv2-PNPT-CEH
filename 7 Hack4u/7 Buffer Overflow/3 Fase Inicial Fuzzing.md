# Fase inicial de Fuzzing y tomando el control del registro EIP

Tags: #BufferOverflow #Fuzzing #RegistroEIP 

En la fase inicial de explotación de un buffer overflow, una de las primeras tareas es averiguar los límites del programa objetivo. Esto se hace probando a introducir más caracteres de los debidos en diferentes campos de entrada del programa, como una cadena de texto o un archivo, hasta que se detecte que la aplicación se corrompe o falla.

Una vez que se encuentra el límite del campo de entrada, el siguiente paso es averiguar el **offset**, que corresponde al número exacto de caracteres que se deben introducir para provocar una corrupción en el programa y, por lo tanto, para sobrescribir el valor del registro EIP.

El registro **EIP** (**Extended Instruction Pointer**) es un registro de la CPU que apunta a la dirección de memoria donde se encuentra la siguiente instrucción que se va a ejecutar. En un buffer overflow exitoso, el valor del registro EIP se sobrescribe con **una dirección controlada por el atacante**, lo que permite ejecutar código malicioso en lugar del código original del programa.

Por lo tanto, el objetivo de averiguar el offset es determinar el número exacto de caracteres que se deben introducir en el campo de entrada para sobrescribir el valor del registro EIP y apuntar a la dirección de memoria controlada por el atacante. Una vez que se conoce el offset, el atacante puede diseñar un exploit personalizado para el programa objetivo que permita tomar control del registro EIP y ejecutar código malicioso.


## Buffer Overflow

____