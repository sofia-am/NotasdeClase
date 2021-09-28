hablamos un poco sobre tabla de simbolos, como funciona y para que sirve

hablando ahora sobre el scope de las variables:

la tabla de simbolos tiene que permitir poner variables en distintos contextos, es decir, podemos declarar una variable con el mismo nombre en distintos contextos (dentro de un for, de un while, etc) y cada vez que salimos de contexto debemos eliminarla.

La tabla de Símbolos 
- incorpora los ID de las variables y funciones
- debe controlar ID, si está inicializada y usada, tipo de dato, contexto.

Opciones de implementación:
- Lista -> tiene dificultades para el manejo de los datos, debemos recorrer muchos lugares
- Diccionario -> es mas practico para encontrar la informacion, es mas lento para agregar datos. no nos permite dar dos definiciones de la misma variable, o para hacerlo debería tener profundidad en cuanto a contexto
- Lista de Diccionarios -> Pensando en hacerlo en Java, tenemos un diccionario por nodo.Lista doblemente enlaza para los contextos y un mapa para cada contexto

Podemos implementar la Lista de Diccionarios como una clase llamada Colecciones

La clase ID deberia tener los setter y getters
 y funcion debería tener una funcion para agregar argumentos 