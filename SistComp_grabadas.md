## ¿Porqué seguimos programando en lenguaje ensamblador?

- eficiencia -> encontrar el mejor desempeño (velocidad, tamaño del programa, organización de los datos)
- poder acceder al hardware del sistema

si queremos saber la organización de la computadora para saber como gestionarlo de la manera mas eficiente entonces tenemos que hacerlo en lenguaje ensamblador
 
## Programador de sistemas y programador de aplicaciones

### Programador de Aplicaciones
Si desea emplear el lenguaje de máquina debe conocer los registros internos accesibles para la manipulacion de datos y direcciones, el repertorio basico de instrucciones y los modos de direccionamiento (como traemos y llevamos datos del procesador a la memoria y como se mueven los datos entre distintos modulos de memoria)

accede a los registros de trabajo para manipular las aplicaciones del usuario
instrucciones, datos y direcciones además de otros elementos 

no accede a la gestión de los recursos de la CPU, de acuerdo con un mecanismo de protección que controla los accesos entre las tareas entre sí, lo deja en manos del sistema operativo

### Programador de Sistemas
Accede a los recursos de la CPU para optimizar su funcionamiento y obtener la máxima potencia, seguridad y rendimiento
Debe conocer el funcionamiento de la memoria virtual, las características de protección del entorno, los mecanismos que permiten la conmutación de tareas, el tratamiento de las interrupciones y excepciones, entre otras cosas

El objetivo es construir un sistema de explotación optimo que sea capaz de soportar todas las aplicaciones previstas como ser:
- organizar el sistema para el correcto tratamiento de las tareas pertenecientes a los diferenes usuarios
- asignar a cada tarea su nivel de privilegio y un sistema de protección adecuado
- organizar toda la memoria y el procesador para que las tareas consigan un mejor rendimiento

### Registros internos para el programador de aplicaciones
```
EAX -> acumulador
EBX -> base
ECX -> contador
EDX -> datos
ESP -> puntero de pila
EBP -> puntero de base (base pointer)
ESI -> indice fuente (snack pointer)
EDI -> indice destino 
```

estos registros son de 32 bits, podemos acceder a menos bits de ese registro con otros nombres como AX, BX, CX DX SP, BP, SI, DI (16 bits), y de ese registro de 16 bits, si accedemos al byte de menor peso se referencia como AL, BL, ... y al de mayor peso como AH, BH, ...

tiene que haber una compatibilidad entre la cantidad de bits que tiene el registro y la cantidad de bits que tiene la memoria a la que estoy accediendo  

## Introduccion ASM
tipos de declaraciones:

1 - Ejecutables
    le dicen a la CPU qué hacer. son instrucciones ejecutables y poseen un mnemónico que despues genera lenguaje de máquina

2 - Directivas de ensamblador
    no son ejecutables, no generan instrucciones de lenguaje de maquina

3 - Macros
    son notación abreviada para un grupo de instrucciones. durante el ensabmlado, cada macro es reemplazada por un grupo de sentencias que representa y es ensamblada en su lugar

Las declaraciones de lenguaje ensamblador se introducen por línea en el archivo fuente

## Movimiento de Datos

ejemplo:
```assembly
MOV ECX, 08h
Start:
    IN AL, 70h
    MOV [EBX], AL
    INC EBX
    LOOP Start
```
EBX contiene la base de una dirección donde empieza una estructura de datos

[EBX] hace referencia al lugar de memoria al que apunta EBX

## Declaración de regiones de datos estáticos
 DB -> declare byte
 DW -> define word

EQU -> valor que permanece constante a traves de toda la ejecucion del programa ya que está embebida en el codigo

los otros 2 son variables, ya que hacen refencia a la ubicacion del dato

## Memoria de direccionamiento

la información de mueve de derecha a izquierda

`mov []`
que tenga corchete significa que no mueve el valor
voy a ir a apuntar a una direccion de registro en esa dirección
mover desde la memoria a otro lado

en esa dirección hay algo y el tipo de ese algo es 

`mov [var]`, ebx -> no pongo el dato en var, sino en la dirección a la que apunta var

- movimiento de datos
- aritmetica/logica
- flujo de control

