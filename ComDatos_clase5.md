# Clase 5

## Capa Fisica

**Objetivo** explicar cómo los protocolos de capa fisica, los servicios y los medios de red admiten las comunicaciones a través de las redes de datos.

Utilizamos la palabra capa fisica para hacer referncia a todos los medios de enlace, se incluye no solo la conexion cableada sino tambien la inalámbrica.

Transporta bit a través de los medios de red.
Modelo por capas: posibilitamos que distintas tecnologias puedan avanzar sin modificar las demás capas. Que puedan avanzar sin dejar de hacer compatibles a las demás capas, además de simplificar el problema a problemas mas simples.

## Capa de acceso al medio
Estamos mas orientados a circuitos electronicos, medios y conectores. De ahí para arriba es software

## Capa de control de acceso al medio (MAC)
Esa direccion fisica está en la placa de red. Subcapa de la capa de enlace de datos.
De ahí para arriba estamos hablando de software, están a nivel sistema operativo, éste se encarga de implementarlo. 

## Sockets

El SO interfacea con los programas por medio de sockets, todos los programas tienen funciones para acceder a ellos.
Por ejemplo hago un programa en C que se encarge de hacer un sistema de e-mail.
Mediante una funcion abrimos un socket para comunicarnos con la pila TCP/IP, y usar el servicio de aplicacion.
Desde el socket le pedimos al sistema operativo servicios del protocolo TCP/IP de la capa de servicio de aplicacion. 
