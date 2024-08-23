# Lectura y escritura de archivos 

Tags: #Python3 

La lectura y escritura de archivos son operaciones fundamentales en la mayoría de los programas, y Python proporciona herramientas sencillas y poderosas para manejar archivos. En esta clase, aprenderemos cómo abrir, leer, escribir y cerrar archivos de manera eficiente y segura.

**Manejo Básico de Archivos**
Explicaremos cómo utilizar la función ‘**open()**‘ para crear un objeto archivo y cómo los modos de apertura (‘**r**‘ para lectura, ‘**w**‘ para escritura, ‘**a**‘ para añadir y ‘**b**‘ para modo binario) afectan la manera en que trabajamos con esos archivos.

**Lectura de Archivos**
Detallaremos cómo leer el contenido de un archivo en memoria, ya sea de una sola vez con el método ‘**read()**‘ o línea por línea con ‘**readline()**‘ o iterando sobre el objeto archivo.

**Escritura en Archivos**
Examinaremos cómo escribir en un archivo usando métodos como ‘**write()**‘ o ‘**writelines()**‘, y cómo estos métodos difieren en cuanto a su manejo de strings y secuencias de strings.

**Manejadores de Contexto**
Uno de los aspectos más importantes de la lectura y escritura de archivos en Python es el uso de manejadores de contexto, proporcionados por la declaración ‘**with**‘. Los manejadores de contexto garantizan que los recursos se manejen correctamente, abriendo el archivo y asegurándose de que, sin importar cómo o dónde termine el bloque de código, el archivo siempre se cierre adecuadamente. Esto ayuda a prevenir errores comunes como fugas de recursos o archivos que no se cierran si ocurre una excepción.

El uso de ‘**with open()**‘ no solo mejora la legibilidad del código, sino que también simplifica el manejo de excepciones al trabajar con archivos, haciendo el código más seguro y robusto.

```python 
#!/usr/bin/env python3 


```