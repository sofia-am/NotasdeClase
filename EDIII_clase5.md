# Clase 5
Jueves 2 de Septiembre

## Repaso interrupciones
La funcion `NVIC_EnableIQR` modifica un registro llamado `ISER` (interruption service?), nos permite habilitar las interrupciones. Entonces cuando llamamos a esta funcion, lo que hacemos es poner en 1 alguno de los bits que pertenece a este registro.

Registro `IPR` nos permite asignarle prioridad a las interrupciones

`NVIC_GetPriority` nos permite conocer el valor de prioridad asignada a la interrupcion especificada
`NVIC_SetPriority` nos permite establecer la prioridad de una interrupcion.

## Systick
Timer integrado dentro del microcrontrolador

El systick y otros registros propios del micro esta en el header `core_cm3.h`

Queremos saber por ejemplo cuando nos toma la funcion retardo, la que utiliza un for con un iterador muy grande.

Hacemos uso de los (pseudo)registros `cycleDelta` (+ `Disassembly` para seguir el codigo en ejecucion) para conocer la cantidad de ciclos utilizados en la ejecución hasta el breakpoint. 

Entre los perifericos (pagina 13 donde tenemos el diagrama de elementos que tiene el micro) tenemos el systemclock

Tiene un solo core, corre linea por linea, el sistema operativo utiliza "paralelismo" para darle tiempo a los distintos procesos para ejecutarse, el elemento encargado de brindar estos tiempos, es el systemclock, que está integrado en el CPU ya que no queda a cargo del fabricante. Puede tener otros relojes integrados o no, que si depende del fabricante, es una decision de diseño.
Por eso se dice que es un periferico "a medias".

Afirmar que es un timer de 10 miliseg no es correcto, ya que depende de la frecuencia de funcionamiento y de los valores de los registros.

Seccion 23.1, pagina 515. **Configuracion Basica**

Tiene un vector de interrupcion propio.

Es un timer de 24 bits que genera una interrupcion cuando llega a 0. Es análogo al `TIMER0` y al `TIMER1`. Se recarga con el registro `STRELOAD`

Cuando se resetea se carga con un valor por defecto que se carga en el registro `STCALIB`, toma por defecto un valor de: 0x000F423F (valor maximo que se puede cargar) -> en decimal: 999999 + 1 = 1000000

Para saber cuanto tiempo le va a tomar: 100000/100e^6 = 0,01 : 10 miliseg

### Algunos Registros Asociados:

`STCALIB` -> se carga un valor de registro, se carga en el booteo. Si queremos que se almacene otro valor, podemos modificar este registro pero modificando el boot.

`STRELOAD` -> lo que vamos a hacer nosotros es modificar **este** registro.

`COUNTFLAG` -> me avisa que el contador del clock llego a 0, se limpia el flag leyendo el registro.

`RELOAD` -> acá va a buscar el valor con el que se tiene que recargar.

`CURRENT` -> 

### Ejemplos Calculos Timer:
(pagina 519, sección 23.6)


CPU Clock = cclk -> frecuencia a la cual está funcionando el clock.
1 c = una cuenta

1/cclk [sg] --- 1 c

 10e^-3     --- x = (10e^-3 . 1c) /(1/cclk)

cclk. 10e^-3. 1c = (cclk .10 .1 c)/ 1e^3

(cclk/100) cuentas -> valor que cargaria en el registro STCALIB

Reload = (cclk/100) - 1

Como mencionamos anteriormente, las librerias asociadas al sysclock se encuentran en `core_cm3.h` . Algunas como:

`SysTick_Type` -> estructura con los registros asociados al systick timer
 
o podemos trabajar modificando simplemente el registro de recarga a través de una funcion que nos provee el header: `SysTick_Config(uint32_t ticks)` -> inicializa el systick timer, su interrupcion, y lo habilita. Definimos el valor de recarga que lo tiene como parametro de entrada `ticks` -> es lo que llamamos cantidad de cuentas que se van a llevar a cabo.
Como salida devuelve 0 si se ha podido ejecutar de forma adecuada la funcion devuelve un 1 si alguna configuración ha fallado.

`STRELOAD = LOAD`

`STCURRENT = VAL`





