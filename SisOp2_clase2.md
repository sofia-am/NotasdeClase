# Sockets

Se usan en modelos cliente-servidor, generalmente los clientes conocen el servidor y saben donde está, y el servidor no lo sabe pero queda a la espera de la comunicación

El proceso le pide al sistema operativo que genere los sockets.

El programador no necesariamente tiene que preocuparse por la generacion de una dirección especifica del lado del cliente a diferenia del servidor que si lo necesita

El cliente se comunica con el server a traves de esa dirección particular por medio del socket

La comunicacion local es que los dos procesos están en el mismo sistema

La comunicacion remota corresponde a procesos que están en sistemas diferentes

La idea de la abstracción del socket es que ambas formas sean iguales (comunicarse con un proceso local o uno remoto)

## Clases de Protocolos

Los protocolos basados en paquetes -> el SO los encapsula en un wrapper y se lo envia al servidor del otro lado

Cuando uno elige el socket elige que protocolo quiere usar

## Estilos de Comunicación

`SOCK_STREAM` comunicacion full duplex con control de errores

`SOCK_DGRAM` no hay secuencialidad en el envio de datos y es posible hacerlo sin conexion

`SOCK_RAW` el SO lo deja de lado y nosotros lo tenemos que implementar desde el lado de la aplicación

## Operaciones con Sockets

Para lectura y escritura podemos utilizar las mismas funciones que con los archivos

### Creación

`int socket(int domain, int type, int protocol)`

tanto el servidor como el cliente tienen que usar una llamada similar

- domain -> `AF_UNIX`
- type ->  
- protocol -> generalmente se usa 0 para que adope el valor por defecto basado en domain y type
- `socket()` -> devuelve un file descriptor, tal como sucede en la creacion de archivos

leemos y escribimos sobre ese file descriptor en nuestro programa, det odo el resto se encarga el SO

## Estructura `sockaddr`

La estructura fundamental del API de sockets de Berkeley se denomina `sockaddr` 

es muy importante saber la dirección del servidor desde el lado del cliente

la dirección sockaddr tiene que ser muy flexible ya que debe ser util para una conexion local como remota donde necesitamos conocer informacion como direccion o puerto

se le hace un casteo para generar la dirección correcta

(filmina 11)

la llamada de apertura del socket necesita la dirección

## Familias de Direcciones

para conexiones unix -> `AF_UNIX`

para conexiones internet -> `AF_INET`

`sa_data` contiene la direccion del socket en caso del unix es ...

## Asociar una direccion al socket

interviene la estructura `sockaddr` que es genérica ya que debe funcionar para varios tipos de sockets

```c
#include <sys/socket.h>
struct sockaddr_un st_servidor;
/* Limpieza de la estructura */
memset( &st_servidor, 0, sizeof( st_servidor ) );
/* Carga de la familia de direcciones */
st_servidor.sun_family = AF_UNIX;
/* Carga de nombre de archivo, asumiendo que
fue pasado como primer argumento al programa */
strncpy( st_servidor.sun_path, argv[1],
sizeof( st_servidor.sun_path ) );

int bind(int sd,
struct sockaddr *my_addr,int addrlen);

int resultado;
resultado = bind( sock_serv,
(struct sockaddr *) &st_servidor,
SUN_LEN( &st_servidor ) );
if( resultado < 0 ) {
perror( "bind" );
exit(1);
}
```

esto se hace tanto en el cliente y en el servidor (servidor: para saber a donde está escuchando) (cliente: para saber a donde tenemos que escribir, a dónde tenemos que asociar la comunicacion)

en el caso del cliente no necesariamente hay que hacer un bind

## Modo Unix sin conexión

cada vez que queremos mandar un dato tenemos que mandarle la dirección de nuevo

juntos con los datos viene la forma de comunicarse con el cliente

## Recepcion sin conexión 
```c
ssize_t recvfrom( int sockfd, void *buf,
size_t len, int flags,
struct sockaddr *src_addr,
socklen_t *addrlen );
```

En el caso con conexion el sistema operativo mantiene levantada la conexión

## Gestionando una conexión

` int listen(int sockdf, int backlog);` define en el filedescriptor del socket que vamos a escuchar cierta cantidad de conexiones simultáneas

vamos a aceptar una cantidad maxima de conexiones simultaneas (ejemplo servidor web), tiene un límite, no son infinitas

`int accept(int sockff, struct sockaddr *addr, ..)`
recibe un file descriptor específico para esa comunicación, ahora el servidor va a recibir y escribir en ese file descriptor, no el del socket

las dos funciones anteriores se ejecutan en el servidor

en el caso del cliente la funcion extra que aparece es la función `connect()`

una vez conectado el cliente debe escribir y leer del file descriptor del socket

