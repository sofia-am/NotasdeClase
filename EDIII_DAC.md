# DAC

conversor digital - analogico

tenemos la salida `Aout` de salida analogica.

Se puede usar con DMA (puedo ir directamente a memoria sin pasar por el core). A través del DMA puedo muestrear una señal analógica del ADC, y volverla a reproducir a traves del DAC.

Puedo programar que cuando el ADC termine de convertir, se genere una interrupcion pero que esta no interrumpa al core, sino que simplemente le avise al DMA que tiene un dato listo para almacenar.

Se los puede llamar "co-procesadores" porque tienen su propia programación y sus propios registros (junto con el Ethernet y los demás celestitos). Trabajan de forma asíncrona.


- Convertidor digital a analógico de 10 bits
- Arquitectura de cadena de resistencia (**no** es el R2R que vimos en digital I)
- Salida bufereada
- Modo de apagado
- Velocidad seleccionable vs Potencia
- Velocidad maxima de actualizacion de 1 MHz -> frec mas bajas nos van a dar menos consumo.

CDA basado en un divisor KELVIN-VARLEY, cierro un switch a la vez, me ofrece distintos niveles de tensión.  

Es importante tener en cuenta que el DAC no tiene un control de potencia en el registro `PCONP`. Simplemente seleccione la funcion alternativa `AOUT` para el pin `P0.26` usando el registro `PINSEL1` para habilitar la salida del DAC
. Solo el hecho de habilitar la salida analogica, equivale a  poner un 1 en el control de potencia. Estoy obligado a setear el pin que voy a usar. No puedo hacer NADA si antes no habilité la salida analogica

## Configuración

1) Energia: el DAC siempre esta conectado al VDDA -> alimentacion del bloque analogico. podía introducir ruido. Debo configurar el pin analogico con `PINSEL` y `PINMODE` (nos sirve poner resistencias de pull up para levantar el nivel de tensión a la salida, por ejemplo)
2) Reloj: en el registro `PCLKSEL0`, seleccione `PCLK_DAC` bits <22:23> (nos permite escalar el clock, nos dice por cuánto voy a dividir el clock).
3) Pines: habilite el pin DAC a través de los registros `PINSEL`. Seleccione el modo de pin para el pin del puerto con DAC a través de los registros `PINMODE`. Esto debe hacerse antes de acceder a cualquier registro DAC.
4) DMA: el DAC se puede conectar al controlador GPDMA.

`PINSEL1` bits <20:21> -> funcion 10: `AOUT`, le da potencia al DAC automáticamente. Va a ignorar todas las configuraciones si no lo encendemos.

Si no utilizamos ni el ADC ni el DAC tenemos que poner las patitas de `VREFP` y `VREFN` vinculadas a VDD(3,3 V) y a VSS, sino la placa no funciona. Esto por default viene así, pero hay que tenerlo en cuenta si modificamos el valor de las tensiones de referencia. Lo mismo con los registros VDDA y VSSA. 

El modulo DAC posee 3 registros importantes: `DACR`, `DACCTRL` y `DACCNTVAL`, pero solo accederemos al primero ya que los ultimos 2 están relacionados con la operacion del DMA 

## Registro `DACR` aka registro de convertidor

Este registro de lectura/escritura incluye el valor digital que se convertirá en analógico y un bit que cambia el rendimiento frente a la potencia. Los bits <5:0> están reservados para D/A en el futuro, para convertidores de mayor resolucion.

Los tiempos de establecimiento acordados en la descripcion del bit BIAS son validos para una carga cuya capacitancia en el pin AOUT no exceda los 100 pF. Si no se cumple esto, no se respetarán los tiempos descriptos en el bit BIAS.

