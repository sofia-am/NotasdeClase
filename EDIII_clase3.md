# Clase 3

`PINMODE` dentro de la estructura del header nos muestra que podemos acceder como un elemento del registro PINCON

Basic configuration: **Seccion 30.1** página 593.

## Caracteristicas eléctricas:

Seccion 11, datasheet (cortito).
Puertos de entrada seteados como digitales.

### Salidas 

Region de niveles de tension:
- Voh: (salida en alto) para Ioh = 4mA
    maximo: Vdd (voltaje de alimentacion)
    minimo: Vdd - 0.4V
    region donde se determina que se está sacando un 1 logico.

El consumo de corriente ocasiona que se vaya disminuyendo el nivel de voltaje a la salida.
- Vol: (salida en bajo)
    0.4V - 0V: region donde se considera un 0 logico.

Fig12: al consumir mas corriente, se disminuyen los niveles de voltaje que entrega el micro.
puedo tener 3.3V al principio, pero a medida que consumo corriente ese voltaje se va degradando. (degradamos el 1 logico)

Lo mismo sucede para el 0 logico, a medida que le pedimos mas corriente de salida a ese puerto, se va alejando del 0V ideal.

Si estamos conectando un dispositivo con el que necesito enviar informacion, entrar en esa zona de incertidumbre es un problema. En cambio si lo usamos para entregar energia a un LED, esa degradacion implica que se va a iluminar menos. 

Corriente que usaremos por general: de 4 a 10 mA.

### Entradas

- Vih: Vdd - Vdd*0.7 -> para las entradas podemos trabajar con ingresos de 0.5V, es decir trabajar con tecnologías TTL y no le van a generar daños al dispositivo.
- Vil: 0.3*Vdd - 0v -> rango donde considera el 0 logico.

Las corrientes para un nivel bajo, trabajamos con un valor tipico de 10nA
Estos valores cambian si trabajamos con resistencias de Pull-Up o Pull-Down internas. Logramos la vida util de la bateria, mejoramos el consumo de potencia.

Las resistencias de pull-up y pull-down se utilizan cuando el puerto está seteado como **entrada**.

Cuanto mas bajo se encuentre del valor de referencia de 3.3V (con resistencias de pull-up activadas), mayor será el consumo de corriente. Podemos conectar entradas que consuman hasta 5V.

`PINMODE<0-9>`
Se asocia de a 2 bits: 
 
 `00` pull-ups
 
 `01` repetitivo -> switchea la resistencia al valor que tenga el pin en esa entrada. está censando constantemente, si hay un 1, se conecta al pull up, si hay un 0 a la entrada se conecta al pull down, si pierdo el valor a la entrada, tengo el valor "guardado" en la resistencia.
 
 `10` ni pull up ni pull down
 
 `11` pull down

Utilizando pines como salidas:

Podemos configurarlo como pin con drenador abierto. No lo vamos a configurar de esta forma.

## Ejemplo MCUXpresso

Proyecto de Debug -> botones propios de depuracion, saltos de linea, introducirnos en funciones, etc. Botones de reseteo.

Tenemos acceso a todos los perifericos que estamos utilizando, por ejemplo GPIO, y nos aparece otra ventana: una con todos los registros asociados a ese periferico.

En la ventana variables tenemos las variables que utilizamos en el programa, y otra de variables globales.

Contamos tambien con la ventana de disassembly donde podemos ver, por cada linea como es el codigo en assembly

Por defecto CMSIS setea el clock interno en 100 MHz

Dentro de los registros, podemos utilizar el que se llama `cycleDelta` para ver, por cada instruccion de disassembly cuantos ciclos le toma cada una. En `cycles` tenemos el tiempo total acumulado.
 
<!-- ctrl + ] = comentarios -->

`1/clock = 1 ciclo`

```c
int main(void){

    uint32_t tiempo;
    uint32_t relojCpu = SystemCoreClock;
/*configuramos pin P2.10 como entrada*/
    LPC_GPIO2 -> FIODIR &= ~(1<<10); 
/*pongo un 3 = 11, desplazado 20 posiciones. Asocio resistencia de pull-down*/
    LPC_PINCON -> PINMODE0 |= (3<<20) 

    while(1){
/*estamos haciendo polling, se puede hacer por medio de interrupciones en el puerto*/

        if(LPC_GPIO2->FIOPIN)&(1<<22))
/*el and es bit a bit, nos permite evaluar si el pin esta en 1, si es asi es true y entra al if*/
        {
            tiempo = 10000000;
        }else{
            tiempo = 40000000;
        }
        LPC_GPIO0 -> FIOCLR |= (1<<22);
        retardo(tiempo);
        LPC_GPIO0 -> FIOSET |= (1<<22);
        retardo(tiempo); 
    }
    return(0);
}
void retardo(uint32_t tiempo){
    for(contador = 0; contador<tiempo; contador++){

    }
}
```
*Clase siguente, aprendemos a utilizar interrupciones y no hacerlo por polling*


