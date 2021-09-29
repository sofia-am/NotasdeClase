# Capa de Enlace (2) (network interface)

## ¿Qué servicios presta la capa de enlace?

- **Framing**: se leen de los bits recibidos por la capa fisica y se estructuran dentro de un _frame_ definido según el protocolo de la capa de enlace.

frame = [Header interfaz][Header red][Header transporte][mensaje][Tail interfaz]

- **Acceso al medio**: define las relas por las cuales un _frame_ será transmitido o no por un enlace.

    la capa de enlace junto con la interfaz son las encargadas de transformar el frame en información transmitible por un medio físico 

- **Transmisión Confiable**: a través de advertencias cuando un paquete no fue entregado correctamente (se utiliza cuando el medio de trnsmisión es no guiado)

- **Detección y correción de errores**
----
Se implementa en las interfaces de red (NIC) de los equipos físicos. Una parte se lleva a cabo en hardware y otras funciones en software.

por ejemplo, mi computadora va a tomar los datos y los transforma en un datagrama (la trama que sale de la capa 3), y a través de interrupciones del sistema el controlador(driver) se va a dar cuenta que tiene que transformar esos datagramas en un frame y del frame directamente transmitirlos al medio físico, codificandolos y agregando el header y el tail. 

## Protocolos Ethernet

es el protocolo que define como se construyen las tramas de capa 2

hay tramas que van por Ethernet II y las IEEE802.3, son los dos protocolos válidos.

[Destino][Origen][Type]

el type nos dice como tenemos que leer el protocolo que viene -> IPv4 utiliza Ethernet II para transportarse

NO podemos utilizar cualquier protocolo ethernet para cualquier capa de red, por lo general las capa 2 y 3 están relacionadas. 

Dijimos que la capa de enlace nos ofrece servicios de detección de errores. Para hacerlo implementa el tail, que a través de de bits de redundancia nos permite saber si hubo errores en la transmisión 

## Redes LAN

### Direcciones fisicas (capa 2)
(direcciones logicas corresponden a capa 3)

Las direcciónes físicas MAC son de 48 bit son "unicas" y están asignadas a cada interfaz de red. Permiten direccionar los paquetes dentro de una LAN 

direccion de broadcast: FF:FF:FF:FF:FF:fF 

_¿Qué significa broadcast?_ que cuando enviamos un mensaje a un nodo, ese nodo se encarga de replicarlo a todos los demás receptores 

la PC1 quiere mandar un mensaje:

PC1 lo envia al switch -> como el switch recibe de direccion de destino una broadcast, copia el mensaje y se lo manda a los demás equipos de la red

_¿Para qué querríamos usar broadcast?_
Supongamos que PC1 le quiere enviar un mensaje a PC2 pero no conoce su dirección MAC, pero si su IP. Entonces va a enviar un mensaje broadcast preguntando a todos los equipos: ¿tu dirección IP es x?, cuando la PC2 responda si, en esa respuesta la PC1 va a conocer la dirección MAC de PC2
   
## Direcciones de red

172.16.0.85/24

Las direcciones de red IP son de 32 bits, identifican a los hosts a nivel de capa de red. 

A través de la mascara de red se puede dividir una direccioin de red de la direccion de host. En una red LAN todos los hots son parte de la misma red. 

La NIC puede tener más de una dirección de red  

la **máscara** me dice cuantos bits pertenecen a la dirección de red -> le hago una AND con la dircción IP y con eso conozco cual es la direccion de red. 

todos aquellos que tengan la misma dirección después de hacer la AND, están en la misma red. 

## Dispositivos de red

- **Router** -> lee hasta la capa 3 y de ahí redirecciona
- **Hub** -> cuando A le envía un mensaje a B, el hub lo reenvía a TODOS sus puertos, por más que tenga la dirección MAC de B.
- **Switch** -> cuenta con una tabla a través de la cual "aprende" que host está conectado a qué puerto. Lo logra porque puede leer los headers de la capa 2 

_¿Qué quiere decir que un dispositivo trabaje en una capa u otra?_ significa que el dispositivo puede leer los headers del frame de dicha capa.
Si un dispositivo es de capa 3 (router), significa que puede leer headers de la capa 2, porque para poder leer el header de la capa 3 necesita haber desencapsulado el header de la capa 2. 

El hub es un dispositivo de la capa física porque no sabe leer direcciones 

## Dominios de colisión y dominios de broadcast

- Un dominio de colisión es un segmento de red compartido por todos los hosts para comunicación bidireccional. En este caso los datos transmitidos por los hosts son susceptibles a una "colisión". Para evitar este fenomeno se utiliza el protocolo CSMA/CD
- En el caso de la comunicación full duplex, no es necesaria la utilización de estos protocolos
- Un dominio de broadcast indica "hasta donde llega" un mensaje broadcast de la capa 2







