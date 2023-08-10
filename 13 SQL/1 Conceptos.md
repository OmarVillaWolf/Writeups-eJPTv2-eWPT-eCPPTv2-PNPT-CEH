# Concepto de base de datos

Tags: #SQL 

## Introducción 

Las bases de datos tienen un gran impacto a nivel empresarial y son considerados entre los mas grandes aportes de la informática. 
	* Agrupar y almacenar todos los datos de la empresa de manera centralizada
	* Compartir datos con áreas de una empresa
	* Evitar redundancia y mejorar la organización de los datos 

## Que son las bases de datos?

Una base de datos es como un almacén donde podemos guardar muchos datos de manera organizada para posteriormente utilizarlos de forma rápida y fácil. Las bases de datos se componen principalmente por tablas las cuales guardan los datos. Cada tabla tiene una o mas columnas en las cuales se almacenan la información sobre cada elemento. 

## SQL

El lenguaje de consulta estructurado conocido como SQL, es un lenguaje que nos ayuda a solucionar problemas específicos o relacionados con la definición, manipulación e integridad de la información que se almacena en las bases de datos. 

## Motores de base de datos 

El motor de la base de datos es el servicio principal para almacenar, procesar y proteger los datos. El motor de la base de datos proporciona acceso controlado y velocidad de procesamiento en la gestión de datos. 
	* Escalabilidad 
	* Alto rendimiento 
	* Alta disponibilidad
	* Facilidad de gestión 
	* Bajo costo 

## Conceptos de tablas y columnas 

Las tablas se refieren al tipo de modelado de datos donde se guardan los datos. Su estructura general se asemeja a una hoja de calculo. Las tablas se componen de:
	* Columnas
	* Registros 

## Tipos de datos 

| **Tipos de datos** | **Definición** |
|---|---|
| Int | Numeros enteros |
| Double | Numeros con decimales |
| Char | Cadenas de texto de tamaño fijo |
| VarChar | Cadenas de texto de tamaño variable |
| TinyInt | Utilizado como boolean |
| Text | Cadena de texto sin tamaño |
| Date | Fecha 999/12/31 |
| DateTime | Fecha con hora 9999/12/31 00:00:00 |

## Llaves primarias (Primary keys)

Una llave primaria o primary key se utiliza para identificar de forma única a cada fila de una tabla. Es necesaria en una tabla cuando se realizan bases de datos relacionales. Se define en una columna de la tabla, la cual tendrá los datos únicos para identificar a la fila. 

## Llaves foráneas o Foreign key 

La llave foránea identifica una columna o grupo de columnas en una tabla que se refiere a una columna o grupo de columnas en otra tabla. Las columnas en las tablas referidas deben ser la clave primaria u otra clave candidata en la tabla referenciada. 

## Diagrama EER

Un modelo de entidad-relación es una herramienta para el modelo de datos, la cual facilita la representación de entidades de una base de datos. Existen tipos de relaciones BD ER:
	* Uno
	* Muchos
	* Solo uno
	* Cero o uno 
	* Uno o muchos 
	* Cero o muchos 