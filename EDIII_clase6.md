# Clase 6

El micro que usamos contiene 4 timers (0,1,2,3)

El timer es un registro de 32 bits, es un contador ascendente. Los ciclos de reloj que le llegan al timer están comandados por un prescaler. El reloj que le llega proviene de un divisor de frecuencia, que llega desde el clck de sistema. Puede ser una división por 1, 2, 4 u 8.

El prescaler es un registro de 32 bits que está asociado a otro registro PR, que cuando su cuenta coincida con el valor de PR, el prescaler se va a resetear. Dicho reseteo va a generar un pulso de reloj que le va a llegar al Timer que entonces ahí va a aumentar su cuenta.

El micro nos provee de 4 registros que podemos utilizar para comparar la cuenta del timer y generar algun evento a partir del mismo. (Match registers)

Tenemos 2 etapas de division -> la del div de frecuencia, y la del prescaler

Cuando se produce el match podemos setear el valor de un pin, sin pasar por el CPU ya que es una funcion propia del Timer. 

Otra funcion que tiene el timer es capturar el valor del timer cada vez que se produce un evento externo a través de uno de los pines, y almacena (hace una copia) el valor del timer en alguno de los registros de capture.  

- Prescaler counter -> prescaler auxiliar, que se reinicia cada vez que se iguala su valor al del registro PR

Al momento de que se resetea la cuenta del prescaler se incrementa en 1 el valor del contador del match register. En el ejemplo del datasheet, cuando el match register llega a su valor maximo se resetean (tanto el match register como el prescaler) y se produce una interrupción

T = (1/PCLK)(PR+1)(MR+1)

Las configuraciones que hay que hacerle al timer es:
- prenderlo
- habilitar el peheripheral clock
- habilitar el pinsel con el modo match
- habilitar la interrupciones si es necesario 

Los pines MAT0, MAT1, MAT2, y MAT3 son pines de salida que nos permiten generar algun evento cada vez que ocurra un match. (los 2 pines se prenden a la vez)

Cualquiera de los match nos van a llevar al mismo vector de interrupción, por eso, dentro del handler tenemos que preguntar cual de los 4 ha sido el match que ha generado esa interrupcion

Cuando configuro el PINSEL en modo match, le asigno a ese pin la posibilidad de prenderse cuando ocurre un evento de match del timer !!

`EMR` -> que evento se genera al hacer match

`MR0` -> valor al que se va a comparar el timer0

`MCR` -> determina que operacion se ejecuta cuando hace match 

`TCR` -> 11 habilito el contador y le hago un reset

**si no generamos una interrupción, no se involucra el CPU** 

-> si no uso los pines de salida puedo obviar setear el PINSEL
