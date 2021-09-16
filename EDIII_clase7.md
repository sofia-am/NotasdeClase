# Clase 7
## TIMER0

- `PCLKSEL0`
- `EMR`-> definimos si sacamos una señal externa despues del match, donde esa señal externa será un toggle.
- `MR0` -> 
- `MCR` -> le digo que no interrumpa, le pido que no llame al timer
- `MCR` -> le digo que se resetee? 
- `TCR` -> incializo el timer

No inicializo la interrupcion para este ejemplo, no habilito con el NVIC

Le damos la funcionalidad de MAT0.0 al puerto 1.28, en principio podemos sacar por todos los pines MAT, y todos sacarían la misma señal.

Se genera una salida de onda cuadrada, exclusivamente por parte del timer0.


 ## Modo Match
### Como configurar al timer para que genere una interrupción:

- habilito la interrupcion del periferico `NVIC_Enable(TIMER0_IRQn)`
- habilitamos la interrupcion (de una de las funcionalidades de ese periferico, por match o por capture, este caso por match) en el registro `Match Control Register` -> `MR0I`  (puedo configurar para cada uno de los match, tiene 4 match). quiero que para su match 0, cada vez que el valor del timer sea igual a ese match, interrumpa.


`INTERRUPT REGISTER`-> cuando lo hagamos con mas de un match, tenemos que ir a este registro a ver qué match generó la interrupcion, pues todos van al mismo vector. (cada timer tiene su handler)

## Modo Capture

El timer va a estar sensando un pin en alguna de las 4 entradas, y cada vez que se produzca un evento, se va a copiar la cuenta que tiene el timer hasta ese momento en alguno de los registros. Además podemos configurar si queremos que se haga alguna interrupcion.

Nos sirve para medir tiempo entre dos eventos, por ejemplo, para verificar un debouncing.


- `PCONP` -> prendemos el timer, siempre está prendido pero es por buenas practicas hacerlo
- `PCLKSEL0`
- `PINSEL3` -> pines 24 y 25 del PINSEL3: modo CAP0.0, va a ser nuestra entrada en la cual uno va a estar esperando un evento
- `CCR`-> capture control register: configuramos el evento que va a disparar la captura: cuando esa entrada tenga un flanco de bajada en nuestro ejemplo.
- `CAP0I` -> habilitamos que una interrupcion cuando ocurra ese evento
- `TCR`-> habilitamos el timer y lo ponemos en reset
- habilitamos la interrupcion por timer0 -> es el mismo vector que para el match, por lo tanto el handler a utilizar es el mismo.

dentro del `#include "lpc17xx.h"` se llama al SystemInit(), no hace falta ponerlo

#

## **Drivers:** 
Herramientas pensadas para usar en un hardware especifico, para otro tipo de microcontroladores no sirven ya que en nuestro caso usamos el driver para las 17xx. Trabajamos con el micro a través de sus drivers. 

Los crea su fabricante cuando su intencion es permitir una interacción entre el hardware y el programador, creando funciones que hagan que sea lo mas simple posible

el driver que vamos a usar va a ser el que está en el LEV: _Driver para LPC1769_

**para ver la configuración de proyecto usando drivers: 19:17 hs aprox**

Los drivers hacen uso de CMSIS para generar código. Dentro de la carpeta de CMSIS ahora tengo una de Drivers, en dicha carpeta tenemos un include y un source, donde si observamos el include, tenemos headers que hacen referencia a cada uno de los periféricos.
Estas son las funciones que vamos a usar para configurar nuestro microcontrolador.

```c
#include "LPC17xx.h"
#include "lpc17xx_gpio.h"
#include "lpc17xx_pinsel.h"

#define PIN_22 ((uint32_t)(1<<22))

int main(){
    
    PINSEL_CFG_Type pin_config
    
    pin_conf.Portnum = PINSEL_PORT_0;
    pin_conf.Pinnum = PINSEL_PIN_22;
    pin_conf.Funcnum = PINSEL_FUNC_0; //funcionalidad GPIO

    /* hasta acá solo le cargamos variable a la estructura tipo PINSEL_CFG_Type, todavía no lo cargamos. el fabricante define estructuras que se van a pasar como argumentos a funciones */

    PINSEL_ConfigPin(&pin_conf); // funcion propia del driver
    GPIO_SetDir(0, PIN_22, 1);   //modifica el FIODIR del puerto que le ponga de argumento

    return 0;
}
```

Por ejemplo la funcion `PINSEL_ConfigPin` toma como argumento un puntero a la estructura y lo carga en los registros a través de otras funciones. 

**PREGUNTAR POR SISTICK Y NVIC | PREGUNTAR POR INTERRUPCIONES POR SOFTWARE**



