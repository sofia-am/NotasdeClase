# Clase Recuperación


Todos los bloques naranjas pueden trabajar mientras funciona el ADC ¿? o DMA 

- Convertidor analógico a digital de aproximacion sucesiva
- Multiplexado de entrada de 8 pines
- Modo de apagado
- `VREFN` a `VREFP` (no tienen que superar los 3,3 V cuando uso el pin de la placa como conversor. Si lo uso como puerto aguanta hasta 5V)
- Tasa de conversión de 200 kHz (o frec de muestreo)
- Modo de conversion de rafaga (burst) para entradas simples o multiples, es una conversion permanente del valor
- Conversion opcional en la transicion del pin de entrada o la señal de coincidencia del temporizador. 

Cada entrada tiene un registro de registro de resultado, y tambien tiene un registro global donde guarda la ultima conversion
Normalmente se usa cuando uso las 8 entradas en modo burst y quiero conocer por separado el resultado de la conversión.

El regulador de 3.3 de la placa no me alcanza a dar corriente

No entendi los 3.3V a que hacen referencia aiuda

Para usar cualquier dispositivo periférico:

1) Encender el periferico
2) Configurando el reloj periferico -> no podemos usar un reloj en Megas, sino en KHz que es en los rangos que trabaja el adc
3) Configurar las funciones del pin -> configurar los pines como entradas del tipo analogico `AD0.x`

`PCLK_ADC`

`PCLKSEL0` and `PCKSEL1` individual peripherical's clock select options (tabla 42)

Valido decir: *"considerando que por defecto este pin toma este valor despues de un reset, no lo configuro"*

### Pines relacionados al ADC

- `VREFN`, `VREFP`
- `AD0.1 AD0.0`
- `VDD VSS`

### Registros asociados al ADC (tabla 531)

- `ADCR` -> es el mas importante, es el registro de configuracion principal 
- `ADGDR` -> global data register
- `ADINTEN` -> interrupt enable register
- `ADR0.0 - ADR0.7`
- `ADSTAT` -> status register, si está en modo burst me esta avisando constantemente que está en `OVERRUN` 

### Registro de Control `ADCR` (tabla 532):
- bit 27 - E, edges. 
- bit 16 - B, burst. 0, el bit START controla cuando se inicia la conversion. 1 está permanentemente habilitado.
- bit 21 es  PND. 1, el conversor esta habilitado, 0, esta deshabilitado
- bits START, 8 modos de operación.
pines `MATx.x` son los que nos permiten comparar 

### Registro de datos individuales `ADDRX`

- bit 30 -> OVERRUN (nos permite saber si el valor en el registro de resultados no fue el ultimo?)
- bit 31 -> done
- bits <4:15> -> resultado

### Registro de habilitacion de interrupciones `ADINTEN`

### Registro de estados `ADSTAT`

- bits <0:7> terminó la conversion
- bits <8:15> se sobrescribió el dato, no es el primogénito, sino que realizó otra conversión y lo sobreescribió

tenemos que ser consistentes en si vamos a levantar informacion del registro global o de los interupciones, an stick to it, porque puede haber desconexión entre el reg general y los individuales y podemos leer información errónea.

`ADTRIM` -> no lo vamos a tocar

## Formas de trabajar:

- Disparo controlado o continuo:
1) burst = 1, disparo continuo
2) burst = 0, disparo controlado cuando quiero por ejemplo una conversion por minuto.
determinamos si vamos a detener el prorama hasta que el dato este disponible poling bit sobre DONE, disparamos una interrupcion para utilizar el dato cuando esté disponible.
 **EN DIGITAL III NO USAMOS POLLING**

*¿cuando leemos en burst = 1? ¿en que registro me fijo?
¿en done?* rta: si uso una interrupción, voy a poder leer el dato en el registro durante la misma. el overrun en general va a estar siempre en 1, porque una vez que hizo la primera conversión, siempre va a seguir sobre-escribiendo el registro de datos.






