Clase 2 
15 de marzo

Modelo relacional

Transacción:
Conjunto de una o mas operacions sobre los datos almacenados en un SGBD que para garantizar la consistencia de los datos deben ejecutarse todas o ninguna de las operaciones que la forman

Atomicidad
Lo primero que tiene que garantizarme un sistema de gestion de base de datos es que las transacciones se ejecutan completamente o no lo hacen para nada

La aplicación cliente es la que le manda operaciones para que haga la base de datos.
primero manda un begin transaction y cuando termina manda un commit para avisar que ha terminado una transacción. si en algun momento la aplicacion decide abortar puede hacer lo que se llama un rollback, entonces la base de datos va a deshacer todo los cambios que realizó desde el begin transaction.

Consistencia
el motor tiene que garantizar que la consistencia se respeta en todos los datos

Aislamiento
La concurrencia debe ser gestionada por el motor, queda para otra clase conocer los algoritmos que aseguran que los datos se modifiquen de forma aislada e independiente

Durabilidad
Si se produce un crash me tiene que asegurar que cuando el motor vuelva a funcionar mantenga todos los datos


Características de las relaciones -> las relaciones se asocian a una tabla
instancia de relacion -> tablas con datos


- los valores son atómicos -> una celda no puede tener un valor que no sea atómico
que algo sea atómico depende del diseño.
 cónyuge + nombre de los hijos en una misma celda -> no es atómico porque estoy mezclando muchos datos en una celda
 fecha -> podría decir que no es atómico porque mezclo dia + mes + año, pero a nivel de diseño puedo decir que es atómico porque es un valor indivisible
 si tengo que parsear la celda para obtener el dato, entonces el dato no es atómico

cuando hablamos de diseño hablamos de decisiones, siempre hay un grado de flexibilidad

- cada tupla es única -> una tupla es una fila de la tabla. (si decimos que lo vamos a guardar en filas)
no podemos tener una fila cuya todos sus valores sean iguales

- los valores de un atributo son todos del mismo dominio -> dominio: todos los posibles valores que pueden tomar mis datos

- el orden de los atributos no es significativo -> el modelo relacional ve las relaciones como un conjunto de atributos

- el orden de las tuplas no es significativo -> idem

Esquema de Relación -> tiene un nombre y un conjunto de atributos asociados a ese nombre
un esquema de relacion es un conjunto de atributos

viendolo como una tabla, cada uno de los atributos es una columna de la tabla

fila-registro-tupla -> sinonimos

establecmos interrelaciones entre las tablas

un conjunto de tuplas con valores corresponde a una instancia de relación

Dependencias Funcionales
restricciones sobre el conjunto de tuplas que puede aparecer en una instancia de relación
es como si dijera "estas combinaciones de tuplas son validas"

ejemplo filmina -> dado un numero de vuelo la hora siempre es la misma porque yo establezco que el vuelo sale siempre a la misma hora
ejemplo auto -> asocio un color a una patente, cada vez que aparezca esa patente en un registro, el color siempre va a aparecer el color



