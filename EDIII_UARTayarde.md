# Comunicacion UART

si la configuracion del receptor está en modo par, el bit de paridad le agrega un bit si la cantidad de bits enviados es impar. 

hasta 16 datos se pueden almacenar en la FIFO, va a ir transfiriendo los datos, de a 8 bits, al registro de desplazamiento que es el encargado de ir sacando de a 1 los bits de informacion a través de `Un_TDX`

Envia esos bits de información a través del DMA

Podemos acceder a los datos que llegan a través del DMA

Podemos configurar si queremos una interrupcion cada vez que llega un byte o a los 4 bytes.

Tenemos que definir a qué frecuencia se da la comunicacion -> (Baud Rate Generator) nos permite dividir la frecuencia que le llega al periferico, y pasa a otro bloque donde se pasa a una 2da división. La idea de estos divisores es ajustar lo mejor posible el funcionamiento de los dos bloques, ajustando también la frecuencia de la comunicación.

`Line Control & Status` -> interrupcion si hubo un error
`IrDA` -> permite generar una autoconfiguracion de la comunicacion

si multiplicamos por 256 (2^8) se desplaza 8 lugares hacia la izquierda
`UnDLM` -> 8bits
`UnDLL` -> 8bits

esta concatenación resulta en 16 bits <UnDLM - UnDLL>, que es el valor que va a estar dividiendo la frecuencia que le llega la periférico

