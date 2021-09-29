# ANTLR Listeners

podemos ir recorriendo el arbol sintáctico por medio de listeners, que van a observar que ocurrió algo y pueden largar una funcion que recepte ese evento.

En nuestro caso nos servirá para recorrer el arbol y agregar elementos a la tabla de símbolos.

¿Cómo recorrer el árbol?
- Durante la construcción (al vuelo) con listeners.
- Con el arbol construido, con visitors.

Uno es una interfaz y la otra una clase de la que podemos extender.

la desventaja de los listeners es que va en un solo sentido, no puedo retroceder. si hay cosas sin resolver, no podemos hacer una nueva pasada.

toda la _compilación_ lo vamos a hacer con el listener, y la generación de _codigo intermedio_ con visitors.

`baseListener.java` extiende de `Listener.java` que es una interface.

hay una superinterface que es `ParseTreeListener`, si no entendemos que hace, vamos a la documentacion de antlr

los `TerminalNodes` son las hojas del arbol, los tokens propiamente dichos.

`getType()` nos devuelve un valor entero que se le asignó al token, no el token en si.

`ErrorNode` -> cuando encuentre un error, nos crea un nodo especializado para encontrar un error, pero no deja de ser un token.

podemos conocer el numero en el archivo `.tokens`

Cuando entramos a cualquier regla no conocemos toda la informacion, sino cuando salimos. Hay acciones que debemos realizar cuando se ingresa al arbol, pero si necesitamos informacion que viene de recorrer todos los nodos hijos o los terminales hijos, lo tenemos que hacer cuando salimos de la regla.

siempre se ejecutan 2 listeners, el `enter<Rule>` y el `enterEveryRule`.

y cuando llegamos a los nodos podemos utilizar `visitNode` y `visitErrorNode`

___no escribir nada en BaseListener.java porque cada vez que ejecutemos el g4 se van a pisar___

lo asignamos a una variable del tipo de la superclase.

lo tenemos que conectar al parser con `addParseListener(<nuestro listener>)`

metodos de entrada -> nos da información de la regla a la que ingresamos, si tenemos reglas muy puntuales, ni siquiera nos hace falta recorrer todo hasta llegar al token

metodos de salida -> nos devuelve toda la información

**un exit es cuando de un nodo vuelvo a un padre**

`@0,0:2` -> declaracion nro 0, caracteres 0 1 y 2

cada vez que entramos a un bloque creamos un contexto nuevo, y cuando salimos lo limpiamos.

tarea: crear y borrar contexto, agregar elementos a la tabla de simbolos


