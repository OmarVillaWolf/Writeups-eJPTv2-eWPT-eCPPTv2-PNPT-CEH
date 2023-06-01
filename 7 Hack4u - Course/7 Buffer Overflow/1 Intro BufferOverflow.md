# Introducción al Buffer Overflow

Tags: #BufferOverflow #Explotacion 

El **buffer overflow** (**desbordamiento de búfer**) es una vulnerabilidad común en software que puede permitir a un atacante ejecutar código malicioso o tomar control de un sistema comprometido.

Esta vulnerabilidad se produce cuando un programa intenta almacenar **más datos** en un **búfer** (zona de memoria temporal para almacenamiento de datos) de lo que se había previsto, y al exceder la capacidad del búfer, los datos adicionales se escriben en otras zonas de memoria adyacentes.

Esto puede permitir que un atacante escriba código malicioso en estas zonas de memoria y sobrescriba otros datos críticos del sistema, como la dirección de retorno de una función o la dirección de memoria donde se almacena una variable, permitiendo al atacante tomar el control del flujo del programa.

Los impactos de un buffer overflow pueden ser graves, ya que un atacante puede aprovechar esta vulnerabilidad para obtener información confidencial, robar datos o incluso tomar el control completo del sistema. Si los atacantes disponen del conocimiento necesario, pueden incluso conseguir ejecutar comandos maliciosos en el sistema comprometido.

En las siguientes clases, se verán ejemplos concretos de explotación de buffer overflow en sistemas Windows, lo que ayudará a comprender cómo los atacantes pueden aprovechar esta vulnerabilidad y cómo se pueden tomar medidas preventivas para proteger los sistemas y las aplicaciones.
