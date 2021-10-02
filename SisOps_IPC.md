# Interprocess Communication

Linux Kernel conceptual architecture

El objetivo es escribir codigo que invoque todas las funcionalidades de kernel

- **System V IPC** -> message queues, shared memory, semaphores
- **Kernel IPC** -> wait queues, signals
- **Net IPC** -> domain sockets
- **File IPC** -> fifo, pipes

los pipes no permiten una comunicacion secuencial de un proceso **relacionado** a otro, ejemplo clásico de `ls | lprint`. 

fifo: similar a los pipes pero nos va a permitir que esa comunicacion sea con procesos que no están relacionados. nos quita una limitación, nos va dar una generalidad, una ventaja. 

memoria compartida: los procesos se van a poder comunicar porque ambos van a poder acceder(escribir/leer) a una zona específica de memoria. cuando lo hace y no debe obtenemos un segmentation fault, memory violation, etc

mapped memory: le aplicamos un concepto de darle un entry point en el file system para obtener ciertas ventajas. nos va a permitir que procesos que no están relacionados se puedan comunicar.

en linux siempre queremos que las interfaces sean a través del filesystem, quizás no necesariamente lo son pero queremos que todo se maneje con esa interfaz, podemos hacer operaciones básicas de archivos.

PIPE

transferencia de datos, tiene una entrada y una salida. es half duplex (unidireccional)

solo pueden usarse con procesos con un ancestro en común -> si creamos un pipe y llamamos al fork, los procesos se duplican. no es como que duplicamos el pipe pero si vamos a tener acceso a ese pipe.

el pipe es un espacio de memoria (64 kbytes) para contener información. cuando hacemos fork duplicamos los recursos y podemos acceder a los datos desde dos lugares, tenemos que hacer un pipe antes de hacer un fork porque ... se pisan los datos (?)

## Crear pipes

int pipe -> systemcall (_man pipe_) como todas las systemcall nos devuelve un código de error

cuando creamos un pipe le tenemos que pasar un arreglo de 2 enteros que el pipe va a usar para cargar los file descriptors que el pipe va a usar a la entrada y a la salida del pipe. 

en el index 0 tenemos que poner el filedesc para el archivo que se va a leer, y en el index 1 para escritura(0utput, 1nput). 

vamos a tener un buffer en una zona de memoria para ese pipe.

```c
int main(){
    char *s, buf[1024];

    s = "hello word\n";

    pipe(fds);

    write(fds[1], s, strenl(s)); //pipe elemento [1] -> podemos escribir

    read(fds[0], buf, strlen(s)); //tenemos un proceso unico que escribe en una entrada de un pipe y lee en el otro extremo
}

```
la funcion pipe va a modificar el contenido de los elementos que se le pasan como argumentos.


