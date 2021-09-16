# Cheat Sheet

- 1 ORed with 'x' is always 1 and 0 ORed with 'x' is always 'x' , where x is a bit
- 1 ANDed with x is always 'x' and 0 ANDed with 'x' is always 0.

# Interrupciones
## Interrupciones por puertos de entrada/salida

- Los puertos 0 y 2 proveen interrupciones por flanco de subida, de bajada o ambos.
- `GPI0` y `GPIO2` comparten la misma posicion en el NVIC con la interrupcion ext3.

### Registros asociados:
- `IntEnR`
- `IntEnF`
- `IntStatR`: tenemos _por pin_ registros para leer el estado en cada uno para saber si ocurrió una interrupcion (del tipo rising edge).
- `IntStatF`: lo mismo pero de tipo falling edge.
- `IntClr`
- `IntStatus`: registro general que indica si hay una interrupcion pendiente en alguno de los puertos GPIO

## Interrupciones Externas

Se trata de una funcion que puedo implementar en 4 pines en el puerto 2 `(P2[10]- EINT0, P2[11]- EINT1, P2[12]-EINT2, P2[13]-EINT3)` que me permiten generar una interrupción.

