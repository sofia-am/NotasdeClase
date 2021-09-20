## ¿Qué significan los argumentos del main en C?

`int main (int argc, char* argv[])`

`int argc` hace referencia a la **cantidad** de argumentos que ingresamos al programa al correrlo en la consola

`char* argv[]` representa el **arreglo de strings** con los valores que ingresamos

por ejemplo:
```c
$ gcc ejemplo.c -o ejemplo
$ ./ejemplo palabra 6 hola chau
$ argc = 5
$ argv[0] = ./ejemplo
$ argv[1] = palabra
$ argv[2] = 6
$ argv[3] = hola
$ argv[4] = chau
```

## ¿Cuales son las partes de la suite de compilacion?

**Preprocesador** toma los `include`, `define`, y todas las macro y las resuelve, y basicamente nos devuelve un bloque grande de código en C.

**Compiler** despues ese codigo C es compilado y se convierte en un archivo de código assembly `.s`

**Assembler** el assembler toma ese codigo assembly y lo convierte en código objeto, que es ejecutable, son los archivos `.o`

**Linker** los archivos objeto `.o` se linkean y el linker devuelve un archivo ejecutable `.out`




