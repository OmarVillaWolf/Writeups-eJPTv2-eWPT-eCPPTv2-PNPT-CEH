# Inyecciones LaTeX

Tags: #LaTexInjection #OWASP #Explotacion 

Las **inyecciones LaTeX** son un tipo de ataque que se aprovecha de las vulnerabilidades en las aplicaciones web que permiten a los usuarios ingresar **texto formateado** en LaTeX. LaTeX es un sistema de composición de textos que se utiliza comúnmente en la escritura académica y científica.

Los ataques de inyección LaTeX ocurren cuando un atacante ingresa código LaTeX malicioso en un campo de entrada de texto que luego se procesa en una aplicación web. El código LaTeX puede ser diseñado para aprovechar vulnerabilidades en la aplicación y **ejecutar código malicioso** en el servidor.

Un ejemplo de una inyección LaTeX podría ser un ataque que aprovecha la capacidad de LaTeX para incluir gráficos y archivos en una aplicación web. Un atacante podría enviar un código LaTeX que incluya un enlace a un archivo malicioso, como un virus o un troyano, que podría infectar el servidor o los sistemas de la red.

Para evitar las inyecciones LaTeX, las aplicaciones web deben validar y limpiar adecuadamente los datos que se reciben antes de procesarlos en LaTeX. Esto incluye la eliminación de caracteres especiales y la limitación de los comandos que pueden ser ejecutados en LaTeX.

También es importante que las aplicaciones web se ejecuten con privilegios mínimos en la red y que se monitoreen regularmente las actividades de la aplicación para detectar posibles inyecciones. Además, se debe fomentar la educación sobre la seguridad en el uso de LaTeX y cómo evitar la introducción de código malicioso.

A continuación, se os proporciona el enlace correspondiente al proyecto de Github que nos descargamos para desplegar un laboratorio vulnerable donde poder practicar esta vulnerabilidad:

-   **Laboratorio LaTeX Injection**: [https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/web90/code](https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/web90/code)


## Comandos 

