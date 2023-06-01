# Creación de nuestro laboratorio de pruebas e instalación de Immunity Debuger

Tags: #BufferOverflow #Explotacion 

En esta clase, estaremos configurando todo el laboratorio para poder practicar nuestros primeros casos de Buffer Overflow.

Para ello, necesitarás instalar las siguientes cosas:

- **Windows 7 Home Premium**: [https://windows-7-home-premium.uptodown.com/windows](https://windows-7-home-premium.uptodown.com/windows)
- **Immunity Debugger**: [https://immunityinc.com/products/debugger/](https://immunityinc.com/products/debugger/)
- **mona.py**: [https://raw.githubusercontent.com/corelan/mona/master/mona.py](https://raw.githubusercontent.com/corelan/mona/master/mona.py)
- **SLMail**: [https://slmail.software.informer.com/download/](https://slmail.software.informer.com/download/)

Asimismo, recordad que es muy importante deshabilitar el **DEP** (**Data Execution Prevention**), de lo contrario los Buffer Overflow que estaremos haciendo no funcionarán.

DEP (Data Execution Prevention) es una medida de seguridad que se utiliza para prevenir la ejecución de código malicioso en sistemas comprometidos a través de un buffer overflow.

DEP trabaja marcando ciertas áreas de memoria como “**no ejecutables**“, lo que significa que cualquier intento de ejecutar código en esas áreas de memoria será bloqueado y se producirá una excepción.

Esto es especialmente útil para prevenir ataques de buffer overflow que intentan aprovecharse de la ejecución de código en zonas de memoria vulnerables. Al marcar estas zonas de memoria como “no ejecutables”, DEP puede prevenir que el código malicioso se ejecute y proteger el sistema de posibles daños. Aún así, esto puede no ser suficiente para impedir que el atacante logre inyectar sus instrucciones maliciosas.
