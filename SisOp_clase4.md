#  Clase 4
### Viernes 27 de agosto.


## Git y Github.

cuando hacemos merge y nos traemos cosas de otro archivi

puede ser que los cambios esten bien espaciados en el tiempo y no generen
conflictos, que los commits esten bien generados en la linea del tiempo

segun como esten acomodados (en la linea del tiempo) puede cambiar de nombre y llamarse un rebase, o un fastforward (me traigo todo y pongo mi commit tipo en la primer pila del stack)

```
git pull <origin> <master> --no --ff no fast forward (no me apila cambios)
---no-merge
git reset -> --hard
git reset <hash> 
git stash (guardame todo esto en un stack)
git stash list
git stash pop
git stash drop
git stash apply <id>
nos sirve (para hacer lo que en el pasado hubiera sido copiar todo y guardar en una carpeta nueva y ponerle "esta es la posta")
```
```
ammend
rebase
git cherry-pick <hash>
```
## Temas de clase

si usamos una libreria estandar, podemos usar -lpam porque el linker ya sabe en que carpetas de linux ir a buscarlo
si queremos levantar una libreria local tenemos que usar el . (que indica directorio actual)

para compilar un projecto necesitamos usar make, que va a buscar un archivo llamado Makefile (extension .mk)
dentro de Makefile podemos hacer un include de un .mk donde tenemos un target donde compilamos todos los objetos que se llamen .c

nos encanta debuggear con printf, cout, etc pero no es muy eficaz, por ejemplo podemos no tener un una interfaz donde ver, tenemos que acostumbrarnos a utilizar otras herramientas.

onchip debuggers, se conecta un chip al sistema embebido y desde una interfaz podemos debuguear de manera externa

## GDB

GDB (GNU Debugger):
nos permite hacer scripts 
`%make CFLAGS = -g` //CFLAGS es una macro, que son los flags para el compilador de c
`gcc -g -c main.c` make CFLAGS le agrega el -g que le va a agregar informacion al debugging (ver filmina 31)

```
make clean
make CFLAGS = -g
$(CFLAGS) -> "el contenido de esta variable"
gdb reciprocal (reciprocal es el nombre del archivo ejemplo)
```
symbol puede ser una variable o una funcion

si no chequeamos condiciones para las variables que el usuario me está pasando, obtenemos erores. 
`SIGSEV` signal segmentation violation
se nos rompió el programa en la linea ... toda esa informacion nos la da el gdb cuando hacemos `run`
podemos hacer `bt`(backtrace) y ver toda la secuencia del programa hasta que se rompió.
./reciprocal 0
el assert nos avisa que se rompió

**NO** podemos dejar el assert en manos del cliente, no podemos dejar que el programa explote. podemos pasarle la opcion de nodebug al compilador y las sentencias de assert no van a ser compiladas.
`NDEBUG`, no se hacen los chequeos del assertion, son para el programador, no son para el usuario. Si te salteás los assert, no ocurre esa terminación abrupta.

podemos ver el stack usando `where` (está como al revés porque es un stack de llamadas FILO first in last out)
podemos navegar en el backtrace haciendo `up2`
o hacer un `print argv[]`
o establecer breakpoints y hacer next para navegar linea por linea

en vez de hacer esto de establecer breakpoints, argumentos apra pasarle a las funciones, y navegar en el programa, en vez de hacer eso caaaada vez que corremos el gbd, podemos hacer el script definido por nostros (plain text) con los comandos que queremos ejecutar 

## Coredumps

el programa estaba corriend, estaba cargado en 
el SO le mando un
el programa termina de forma abrupta, no sabemos cuando llega la señal que causó el error

el cordump nos va a servir para hacer post-mortem.

where are core dumps stored?
son la imagen de la ram del sistema cargado en un instante
nos permite tener toda la traza del programa hasta que se rompió

`man coredumpctl `

## Static Libraries

an archive or static library es simplemente una coleccion de object files almacenadas como una single file
basicamente una static library es una API estática: provee funciones utiles, esconde detalles internos

el linker tiene que buscar en el archivo los objetos que necesita de manera particular, y crea un enlace con el objeto que vos necesitas en tu programa.
si queremos usar objetos, usamos una libreria, el linker va a darnos acceso a esos simbolos.

el comando `ar` nos permite una libreria estatica.
cr flag le dice a ar que cree el archivo.

para linkear el archivo cuando compilamos decimos `-ltest` (porque test.c se llama el archivo)

**LINKING ORDER MATTERS**

primero tengo que poner el objeto porque el compilador necesita analizar el objeto para saber qué dependencia necesita. 

`ar -t` lista los archivos con los que armamos la libreria




