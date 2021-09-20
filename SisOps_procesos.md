# Procesos

## ¿Que es un proceso?

Es una instancia de un programa que está corriendo, que está en ejecucion.

Generamos un ejecutable cuando compilamos de tipo ELF. Cuando ejecutamos un programa, existe una entidad llamada el loader que se encarga de cargar un programa y lo convierte en un proceso.

Un programa es ese binario que tiene la capacidad de ejecutarse, en cambio un proceso es un programa que está en ejecucion. Es como hacer la analogía entre clases e instancias, podemos tener 10 procesos que corresponden a un mismo binario.

_"a running instance of a program is called a process"_

En linux existen los PID compuestos por 16 bits(es decir, existe un límite), son unicos para cada proceso. 
- Ese ID se asigna por el kernel de manera secuencial. 
- Cada proceso tiene un proceso padre

**systemd** -> es el primer proceso que se ejecuta. Es el proceso del sistema que levanta todo el resto, todos los servicios: ethernet, wifi, monitor, teclado, etc. Es el padre de todos los procesos. (leer en freedesktop.org)

- `pstree` nos deja ver en forma de estructura de arbol.
- `ps -A`
- `ps` nos muestra que procesos estan corriendo en el sistema
- `-o` la opcion o nos permite filtrar los campos, nos sirve para limtiar la informacion que queremos loguear

## Process Scheduling

Podemos darle prioridad a los procesos. (existe alguna relación entre esto de asignarle prioridad a una interrupcion como hacerlo a un proceso? los procesos tambien se tratan como interrupciones?)

`nice -n 10` -> modificamos la prioridad para que permita que el sistema trabaje con otros procesos

### Process Managment 
- `fork()`
- `wait()`
- `exit()`
- `exec()`

Un proceso no es una iterrupcion, una interrupcion va a tener un handler, un espacio de codigo que se va a encargar de reaccionar a esa interrupcion. Es asíncrono. 

Las interrupciones son señales que me permiten comunicarme con el hardwar. Las interrupciones pueden interrumpir (valga la redundancia) la ejecucion de un proceso. 

El **scheduler** va a tomar decisiones basadas en prioridad, se trata de políticas. Va a penalizar (no elegir) ejecutarlo versus otros procesos de mayor prioridad. 

## Printing de Process ID
- `getpid` retorna un objeto tipo `pid_t` que es un tipo que ya viene definido 
- `getppid` retorna ID del proceso padre 

### ¿Como creamos nuevos procesos?

- `system()` -> mas insegura, ineficiente pero mas simple
- `fork()` -> mas flexible, mas rapida y mas segura.

### Usando `system()`
Hace lo mismo que hacemos por consola, y system se encarga de ejcutarlo dentro de nuestro programa. El problema es que utiliza (invoca) una Shell nueva. Es ineficiente porque hay una redirección: nuestro programa está invocando esa shell, que está ejecutando ese comando para devolvernos un dato.

Si es un programa que se redistribuye en muchos sistemas, no sabemos que shell se va a estar ejecutando. No podemos asegurar que nuestro programa funcione como queremos.

### Usando `fork()`
Un proceso corre `fork` para crear procesos. Nosotros vamos a tener un proceso, que tiene una linea de ejecucion, en un momento se ejecuta el fork y vamos a tener dos lineas de ejecucion: el camino del proceso original (padre) y otro para el que acabamos de crear(hijo). Hay un duplicado de procesos, es decir que hay una **copia**.

A partir de ese momento, los procesos padre e hijo se van a ejecutar concurrentemente. En distintas situaciones vamos a tener paralelismo. Vamos a tener la necesidad de comunicacion y sincronizacion. 

Ahora el scheduler va a tener que ejecutar independientemente a los dos procesos, no tenemos garantías de cuanto tiempo va a tomar ejecutarlo, ni en que orden se van a ejecutar.

Se clona:
- Program Text -> lineas de codigo
- Stack 
- PCB -> process control block
- Data 

Después del fork, los dos programas están ejecutando el mismo programa. EL padre inicialmente era instancia de un programa, el hijo cuando lo clona, tambien es otra instancia de ese programa.

Fork retorna -1 cuando algo sale mal (por ejemplo no hay mas procesos, no tenemos permisos, etc) 

**SIEMPRE TENEMOS QUE HACER HANDLING DE ERRORES**

Si sale bien: nos devuelve el ID del proceso hijo. 

```c
ret = fork();
switch(ret)
{
    case -1:
        perror("fork")
        exit(1);
    case 0:  //child 
        <code for child>
        exit(0);
    default: //parent
        <code for parent>
        wait(0); //espera a que termine de ejecutarse el hijo
        <...>    //solo continua ejecutandose el padre
}
```
Un padre puede querer esperar a que los procesos hijos terminen.
*Ejemplo:* shell esperando que se terminen operaciones.

`wait()`
- Se bloquea hasta que termina un hijo
- Retorna el process ID del hijo
- O retorna -1 si no existen hijos

`waitpid()`
- Se bloquea hasta que termina un hijo con un ID en particular

macro: `WEXITSTATUS` -> nos devuelve con que exit code terminó
(+ otras macros, ver filminas)

***El objetivo de esto es poder construir una terminal propia***