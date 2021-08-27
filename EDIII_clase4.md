# Clase 4

## Jueves, 26 de agosto

* //nota: no copypastear el codigo directamente porque le deben faltar ; y comentar bien.

`NVIC` -> menor latencia en el manejo de interrupciones, pierdo menos tiempo 
ejecutando codigo pra salvar contexto y bajar flags

las interrupciones se hacen por nivel, podemos definir si queremos que sea
una interrupcion por flanco de subida o por flanco de bajada, o por ambos.
(algo que no se podia configurar en el puerto RB0 por ejemplo)

Port0 y Port2 se comparte el msmo vector de interrucion para interrupciones x¿externas

registros asociados a manejar interrupciones:
- intEnR - me permite habilitar las interrupciones si quiero que se interrumpa por flanco de subida
- intEnf - falling edge
- intStatR - registro que guarda el estado de las interrupciones por flanco de subida
- intStatF -
- intClr - limpio el registro de interrupciones (solo escritura)
- intStatus - me permite saber si el que genero la interrupcion es GPIO0 o GPIO2

despues de saber que puerto genero la interrupcion necesito saber en que pines
se generó la interrupcion, ya que cualquier pin de ese puerto puede generar la interrupcion
ya que cualquier sea el pin que lo genero, van a ir a la misma posicion del vector de
interrupcion

tengo que empezar a preguntar cual fue el pin:
P0.0 -> fuiste vos?
else: ...
solo pregunto por aquellos pines que se que van a generar interrupciones

escribo un 1 en el registro IOIntClr -> limpio el flag de la interrupcion 

trucazo,utilizar funcines para las configuraciones, ejemplo confGPIO()
```c
void configGPIO(){
	//interrupcion por flanco de subida
	LPC_GPIOINT -> IO0IntEnR |= (0<<15); //ponemos un 0 en la pos 15 de ese registro//tabla 9.5.2
	LPC_GPIOINT -> IO0IntClr |= (1<<15);

/* lo que pide el fabricante es que cada vez que toquemos los registros, se genere una limpieza
del bit de los flags de interrupcion, hacemos todas las configuraciones y antes de habilitar
la interrupcion, hacemos un clear del flag del pin correspondiente al pin que va a generar la
interrupcion */

	NVIC_EnableIQR(EINT3_IRQn);
	return;
}
```
macro definida por CMSIS que me permite habilitar el handler de la interrupcion.
como argumento va el nombre del handler de la interrupcion correspondiente, sabemos que las interr
por puerto 0 o 2 van al EINT3
ver tabla 640(exceptions), además de ver el numero de la interrupcion dentro de la tabla de vectores
podemos ver el nivel de prioridad, a medida que el valor se acerca a 0, la prioridad se aumenta.

por ejemplo, systick tiene una prioridad configurable

ver tabla 50 -> de ahi sacamos el argumento para la macro `NVIC_Enable `

IRQn -> servicio de pedido de interrupcion

```c
void EINT3_IRQHandler(){ 
//ya estan definidas, tabla 654 CMSIS functions for NVIC control
	
/*pregunto a cada pin para saber quien genero la interrupcion como solo habilitamos la interrupcion para un pin(15), solo le preguntamos a ese.*/

	if(LPC_GPIOINT -> IO0IntStatR &(1<<15)){

		inte++;
	}
	LPC_GPIOINT -> IO0IntClr |= (1<<15);

/*el handler no permite parametros en el argumento de entrada, todas las variables que queramos usar
tienen que estar definidas de manera global*/
}
```

dijo unas cosas de accesos privilegiados a algunos registros pero no entendi cuando/porque se usan.

herrmienta de MCUXpresso: 
- **GPIOINTMAP**, nos permite conocer el valor de los registros asociados a 
las interrupciones, podemos chequear si hicimos bien la configuracion,
podemos poner un breakpoint en la sentencia if de evaluacion para poder leer el registro y saber si
esta en 1 o 0 (es decir si cumple con la condicion que le pusimos)
- **Global Variables:** podemos aprovechar que tenemos esta ventana para utilizar variables auxiliares
que nos ayuden al momento de debuggear. (ver ventana de plot, para ver de manera grafica los valores
que va tomando una variable a lo largo del tiempo, por ejemplo el profe mostró como se modificó la var
que corresponde al pulsador)

si una interrupcion está pendiente, podemos, mientras handleamos la de mayor prioridad, deshabilitar
a la pendiente sin entrar al handler de la misma (nosé, preguntas que hace el tomi magnelli)


## Interrupciones externas: 

tabla 6, seccion 3.2
hay un diagrama en pagina 6.
tabla 4, pin allocation table, podemos ver en que pines estan las funcionalidades de interrupcion
las interrupciones son funcionalidades que se le dan a los pines, tal como le damos funcionalidad
de GPIO, se determinan con PINSEL

EINT0, EINT1, y EINT2 tien su propio vector de interrupcion ---> son interrupciones externas

diferencia entre interrupcion por puerto e interrupcion externa:
interrupcion por puerto: pin seteado como GPIO
interrupcion externa: pin seteado como EINT0, EINT1, EINT2, EINT3 .... (son como el RBO del PIC?)
(*** confirmar si es asi ***) 
 
registros asociados a interrupciones externas:
- EXTINT - flags
- EXTMODE - me permite identificar si esa interrupcion se va a generar o por nivel o por flanco
- EXTPOLAR - si configuro que sea por nivel, determino si va a ser por nivel bajo o por nivel alto,
y si defino por flanco, con extpolar defino si es por flanco de subida o de bajada.
```c
configExt(void){
	LPC_PINCON -> PINSEL4 |= (1<<20); //habilito funcionalidad interr externa
	LPC_SC -> EXTMODE |= 1; //habilito interr por flanco
	LPC_SC -> EXTPOLAR |= 1; //defino interr por flanco de subida
	LPC_SC -> EXTINT |= 1; //limpio bandera de interr externa, se limpia CON UN 1!
	NVIC_EnableIQR(EINT0_IQRn);
```
_¿si fuera EINT3 seria el mismo handler que para la interrupcion por puerto?_

si, si fuera el caso GPIO como en el ejemplo anterior, tendriamos que además de preguntar
en qué puerto se dio la interrupcion(ya que se comparten entre el puerto 0 y el 2, creo)
, preguntar tambien si es una interrupcion por puerto ó una interrupcion externa.
	return;	
}

***tarea: leer systick, hacer las tareas.***
