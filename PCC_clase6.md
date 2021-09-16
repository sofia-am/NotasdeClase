# Clase 7 de Septiembre

Análisis sintáctico ascendente - desde las hojas hasta la raiz

Las acciones son desplazar y reducir

1) desplazar es llevarse un token hacia la izquierda
2) si no podemos desplazar, reducimos, lo que implica que aparece una regla s en el lado derecho.
3) podemos reemplazar una expresión por la regla s, cuando dicha expresión es su definición. `(s)s`


```
s : '(' s ')' s 
-> la primer s nos permite anidar parentesis y la segunda nos permite poner uno detrás del otro.
```
cuando no puede reducir consume tokens, pero cuando no puede aplica la regla vacía.

cuando encuentra un token que es comienzo de una regla, puede seguir desplazando

todas las reglas s aparecen despues de reducir un vacío.

## Operadores logicos

regla:

```
// operacion aritmetico logica, se llama una sola vez
oal : termino t
    ;

termino : factor f
        ;

t : SUMA  termino 
  | RESTA termino
  |
  ;

factor : ENTERO 
       | ID
       | PA oal PC
       ;

f : MULT factor f
  | DIV  factor f
  |
  ;
```

van a tener que extender a secuencias de and or, comparaciones tipo mayor o igual, mayor menor etc




