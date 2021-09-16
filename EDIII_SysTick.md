# SysTick

Temoprizador de sistema, su fncion es generar una interrupcion a intervalos constantes.

En general es de 10 ms siempre y cuando la frecuencia sea de 100 MHz

- El software puede utilizarlo para implementar una funcion de retraso de tiempo. 
- Podemos ejecutar algunas tareas especificas periodicamente.
- Leer entradas externas con un intervalo regular de tiempo

El NVIC monitorea y maneja todas las solicitudes de interrupcion.

Para las interrupciones el handler se llama con SysTick_Handler()

Contador de 24 bits que se decrementa y avisa a través de una interrupcion.

SysTick Interrupt Time Period = (SysTick_LOAD + 1) x Clock Period.

## Registros asociados:

`SysTick_CTRL` 
- `<0> TICKINT`: Indica si contar hasta 0 hace que el estado de la excepción SysTick cambie a pendiente. 
    - 1: contar hasta 0 cambia el estado de la excepcion a pendiente. 
    - 0: contar hasta 0 no afecta el estado de la excepcion systick.
- `<1> ENABLE`: Indica el estado habilitado del contador SysTick. 
    - 1: funcionando, 
    - 0: deshabilitado.
- `<2> CLCKSOURCE`: Indica la fuente de reloj. 
    - 1: usa el reloj del procesador
    - 0: usa el reloj de referencia externo opcional
- `<16> COUNTFLAG`: Indica si el contador ha contado a 0 dede la ultima lectura de este registro.
    - 1: el temporizador ha contado a 0
    - 0: el temporizador no ha contado a 0

`SysTick_LOAD`
Es un registro de valor de contador de (re)carga. Proporciona el valor de ajuste para el contador
- `<23:0> RELOAD`: El valor de 24 bits para (re)cargar en el registro de valor actual (SysTick_VAL) cuando el contador llega a 0.

`SysTick_VAL`
- `<23:0> CURRENT`: Valor del contador actual. Este es el valor del contadore en el momento que se muestrea.

`SysTick_CALIB`
contiene un valor que es inicializado por el codigo de arranque con un valor de fabrica.


## Como inicializar el SysTick:

- Como los registros de Reload y Current Value no están definidos en el reinicio, el codigo de configuracion de SysTick debe escribirse en una secuencia determinada:
    - Deshabilitar el modulo SysTick
    - Programar el registro de recarga
    - Borrar el registro de valor actual
    - Habilitar el modulo SysTick
    - Configurar la interrupcion en el periferico, y habilitar en el NVIC
