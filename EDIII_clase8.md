# Clase 8

**Aproximaciones sucesivas:** Significa que va partiendo el valor de referencia muchas veces según si el valor medido es mas o menos grande que la mitad, lo hace _64 veces_

Siempre tarda el mismo tiempo para hacer la conversión analogico-digital 

No hay que confundir la frecuencia de funcionamiento con la frecuencia de muestreo
 
La frecuencia de funcionamiento o de trabajo es la frecuencia que le llega al ADC y la que lo hace funcionar, en cambio la frecuencia de muestreo es la frecuencia maxima a la que el ADC puede tomar muestras para la conversión 

Si tengo 13MHz de frec de trabajo, el ADC necesita 65 ciclos para convertir 200KHz

- En modo no burst necesita de 65 ciclos
- En modo burst necesita de 64

 _¿Puedo muestrear a mayor frecuencia que 200 K?_ No, para hacerlo necesitaría una frec de funcionamiento mas grande


## **`AD0CR`**
- `SEL`: si utilizamos el ADC en modo burst podemos poner muchos 1s en este registro de 8 bits para elegir los canales que se van a muestrear, pero si lo hacemos en modo unico no, tenemos que tener **un solo canal** seleccionado a la vez.
- `CLKDIV`: tiene asociado internamtente un divisor de frecuencia, además del divisor de frecuencia del clock que le llega al periférico, que termina dividiendo lo que llega al periferico por el valor de este registro + 1
- `BURST`:  1-> genera la conversión en rafaga
- `PDN`: habilita el ADC 
- `START`: define los distintos tipos de inicio de funcionamiento
    - 000 -> no start: **esto se usa en modo rafaga** ya que el mismo periferico define cuando se inicia la conversión (una detras de otra)
    - 001 -> le pido por soft que inicie la conversion
    - 010 -> inicia la conversión por un cambio en un pin externo. para usar ese pin con esa funcion lo tengo que configurar en pinsel 
    - 100 -> inicia la conversión cuando el timer hizo match y avisa por un pin externo 
- `EDGE`: definimos si se inicia la conversion con un flanco de subida o bajada en algun pin externo 

## **`AD0GDR`**
- `RESULT`: el resultado se almacena en este registro a partir del bit 4, es decir, para obtener el resultado tenemos que desplazarlo 4 veces hacia la izquierda
- `CHN`: nos dice que canal generó la muestra 
- `OVERRUN`: avisa si hubo overrun
- `DONE`: avisa que se completo la conversion

**accuracy vs digital reciever**: cuando utilizamos un pin para el ADC tenemos que no solo modificar su funcion en PINSEL sino que tambien ponerlo en modo _neither pull up nor pull down_ porque si tenemos alguna configurada va a ocasionar errores en la medicion del valor 


Para modificar la frecuencia de muestreo podemos:
- modificar el divisor del clock que le llega al periferico
- modificar el divisor de frecuencia propio del ADC

Realizar la 2da no está recomendado porque si genero un "delay" muy grande, el capacitor que tiene el ADC se puede ir descargando y puede ocasionar que tome mediciones incorrectas.

Lo que podemos hacer (mejor) es utilizar los tiempos largos que nos brinda el modulo de Timer, y dejar que siga trabajando con una frecuencia de trabajo alta  mientras uqe el timer es el que inicia la conversión

