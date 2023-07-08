# Lectura de archivos especiales

Tags: #Linux 

```bash 
❯ cat /home/omar/-                          # Forma de leer los archivos con caracteres especiales como '-' pasandole la ruta absoluta
❯ cat /home/omar/*                          # Forma de leer los archivos con caracteres especiales
❯ cat ./-                                   # Forma de indicar que queremos leer un archivo especial '-', indicando que se encuentra en el directorio actual
❯ cat 'spaces in this file'                 # Con '' podemos leer archivos que su nombre tenga espacios, tambien lo podemos hacer haciendo click en TAB para autocompletar
❯ cat spaces\ in\ this\ file                # Se hace esto cuando tu archivo cuenta con espacios y debemos 'escapar'
❯ cat s*                                    # Queremos leer un archivo que empieza con s y tiene una cadena de caracteres (*=wildcard)
```

```bash 
❯ grep -r "\w" 2>/dev/null | tail -n 1      
	# r = forma recursiva 
	# w = palabra 
	# tail sirve para colocar en este caso la ultima linea del codigo 
	# n = numero de la linea que queremos ver, lo del dev/null es porque hay mucha informacion que no tenemos permiso y asi logramos que no nos salga.
❯ grep -r "\w" 2>/dev/null | tail -n 1 | awk '{print $2}' FS=':'            # Filtramos y miramos la ultima linea pero ahora le decimos que queremos quedar con el segundo argumento tomando como delimitador ":" (FS=delimitador)
❯ grep -r "\w" 2>/dev/null | tail -n 1 | cut -d ':' -f 2                    # Filtramos y miramos la ultima linea, para despues cortar con el delimitador ':' y quedarnos con el segundo argumento (d=delimitador, f=file o argumento)
❯ grep -r "\w" 2>/dev/null | tail -n 1 | tr ':' ' ' | awk '{print $2}'      # Filtramos y miramos la ultima linea del codigo para despues sustituir ':' por espacios y al final nos quedamos con el segundo argumento (tr=aplicar sustituciones)
❯ grep -r "\w" 2>/dev/null | tail -n 1 | tr ':' ' ' | awk 'NF{print $NF}'   # Hacemos lo mismo que arriba pero referenciamos que nos queremos quedar con el ultimo argumento de la linea con NF
```

