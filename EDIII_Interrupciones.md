# Interrrupciones
### Clase teórica de Julio Sanchez

Manejo vectorizado de las interrupciones

Cada interrupcin va a tener una direccion que le indica a que direccion de memoria debe saltar para manenjar esa interrupcion.

Periféricos, I/O pins. 

Diferencia entre excepciones e interrupciones:

- Excepciones: son aquellas que corresponden al Core. Éste se comunica con el NVIC para que genere las interrupciones (de sistema). El timer de sistema genera una excepción.
- Interrupciones: hablamos de interrupciones para aquellas que ocurren a partir de periféricos y puertos de entrada/salida.
    - Systick timer. (IRQ)
    - NMI (interrupcion no enmascarable)
    - Periféricos (internos a la pastilla)
    - EXTI Controller: puertos de entrada/salida. El arm tiene un controlador que nos permite lograr un periodo de latencia para que las perdidas de informacion sean mínimas.

Admite 35 interrupciones

Cada registro tiene 5 bits (32 opciones) que me permiten darle un nivel de prioridad

Puedo generar interrupciones por software

**Vectorizadas**: ancho 8 bits (1 byte), cada palabra ocupa 4 bytes.

Las direcciones a las que salta ya vienen programadas de fábrica:
Desde la 0x01 hasta la 0x3C corresponden a las interrupciones de sistema (excepciones), de la pos 0x40 hasta la 0x74 corresponden a las interrupciones

   - 0x3C SysTick_Handler
   - 0x08 NMI_Handler

y en estas direcciones va a levantar la dirección donde se encuentra el handler donde está la rutina de servicio

Cuando llega una interrupción queda, en pendiente mientras "stackea" la información de contexto del programa, en un stack pointer descendiente. Cuando termina de atender la interrupcion, hace un unstacking para recuperar conexto, levantandolo de abajo hacia arriba.

Si viene una interrupcion de mayor prioridad que la que estoy atendiendo en el momento, vuelve a pasar lo mismo, se stackea el contexto de a ejecución del programa durante la atencion de la interrupcion, y pasa a atender la otra.

Está *pending* mientras salva contexto, y en *active* mientras atiende la interrupción.

En el registro de prioridades, los primeros 2 bits son para definir la prioridad, y otros 2 para definir una subprioridad


