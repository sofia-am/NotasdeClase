# Clase 5

## Capa Fisica

**Objetivo** explicar cómo los protocolos de capa fisica, los servicios y los medios de red admiten las comunicaciones a través de las redes de datos.

Utilizamos la palabra capa fisica para hacer referncia a todos los medios de enlace, se incluye no solo la conexion cableada sino tambien la inalámbrica.

Transporta bit a través de los medios de red.
Modelo por capas: posibilitamos que distintas tecnologias puedan avanzar sin modificar las demás capas. Que puedan avanzar sin dejar de hacer compatibles a las demás capas, además de simplificar el problema a problemas mas simples.

## Capa de acceso al medio
Estamos mas orientados a circuitos electronicos, medios y conectores. De ahí para arriba es software

## Capa de control de acceso al medio (MAC)
Esa direccion fisica está en la placa de red. Es una subcapa de la capa de enlace de datos.
De ahí para arriba estamos hablando de software, están a nivel sistema operativo, éste se encarga de implementarlo. 

## Sockets

El SO interfacea con los programas por medio de sockets, todos los programas tienen funciones para acceder a ellos.
Por ejemplo hago un programa en C que se encarge de hacer un sistema de e-mail.
Mediante una funcion abrimos un socket para comunicarnos con la pila TCP/IP, y usar el servicio de aplicacion.
Desde el socket le pedimos al sistema operativo servicios del protocolo TCP/IP de la capa de servicio de aplicacion. 

Por ejemplo tener un socket escuchando un puerto (software) a través del cual vamos a recibir y enviar información. Todos los sistemas operativos ofrecen una interfaz socket para hacer uso de los servicios de la pila TPC/IP.

 ## Componentes Fisicos
NIC, interfaces y conectores, materiales y diseños de cables, todas esas cosas deben estar estandarizadas.
El estandar hace posible que haya fabricantes, pues asi se consolida la comunicacion, permite que haya compatibilidades.

Deben cumplir con estandares de:
 - componentes fisicos
 - codificacion
 - señalizacion

### Codificacion
Convierte la secuencia de bits en un formato reconocible por el siguiente dispositivo en la ruta de red. En un mismo canal mejoramos la posibilidad de que la informacion llegue sin errores, ejemplo codificacion Manchester 0: transicion alto a bajo, 1: transicion bajo a alto. En el destino es mas facil leer una transicion que un voltaje cd que viene con interferencia.
Para velocidades mas altas se necesitan codificaciones mas complejas que van corrigiendo la informacion a medida que viaja por el medio.

*la codificacion ayuda a reducir los errores en la transmision*

### Señalizacion
Como se representan los valores de bits en el medio fisico.
Tiene que ver con una modulacion de una señal digital por medio de una portadora analogica por medio de una banda base.
En la fibra optica existe algo llamado dispercion, entre el pulso y otro, un pulso que dura tanto es un 1, y un pulso de otra duracion es un 0, cuando hay dispersion no me permite distinguir un 1 de un 0.


Latencia -> cantidad de tiempo, incluidos los retrasos.

Rendimiento -> la medida de la transferencia de bits a traves de los medios durante un periodo de tiempo determinado

Capacidad de trasnferencia util-> `goodput = rendimiento - sobrecarga de trafico`. La sobrecarga de trafico son todos los bits necesarios para establecer la comunicacion que se van agregando en la pila tcp/ip

## Cableado de cobre

### Limitaciones


### Par trenzado sin blindaje

interconecta hosts con dispositivos de red intermediarios
 
 1- la cubierta exterior proteje los cables de cobre del daño fisico
 2- 


### Par trenzado blindado 
no son afectados por la interferencia

### Cable coaxial
coaxial -> tienen el mismo eje. 

### Cableado UTP
Trenzados, no usan blindaje, lo que tiene para evitar la diafonia es la cancelacion ya que los cables utilizan polarizacion opuesta, son bucles de corriente. Como son variables los campos que se generan son los mismos pero en direcciones opuestas entonces los campos magneticos se cancelan. Tienen longitud opuesta, evita que se afecten los cables entre si.

Los cables ethernet usan 2 de los 4 pares. Funciona modo full duplex, los otros funcionan para trabajar con otros protocolos por ejemplo VoIP.

Hay 3 categoria
- Categoria 3
- Categoria 5 y 5e (hogares)
- Categoria 6 (datacenters)

### Tipo de medios de fibra

Fibra monomodo (SMF)
- Nucleo pequeño
    
Fibra multimodo (MMF)



### Tipos de medios inalambricos
Los estandares de la industria IEEE y de telecomunicaciones para comunicaciones de datos inalambricas cubren tanto la capa de enlace de datos de la capa fisica.

- Metodos de codificacion de datos a señales de radio
- Frecuencia e intensidad de la transmision}
- Requisitos de recepcion 
- 

### LAN 

## Capa de enlace de datos

Capa 2 OSI, es el responsable de las comunicaciones entre las tarjetas de interfaz de reddell dipositivo final.
 - permite que los protocolos de capa superior accedan a los medios de capa fisica
 - encapsula los paquetes de capa 3 (IPv4 e IPv6) en tramas de capa 2
 - tambien realiza la deteccion de errores y rechaza las tramas corruptas

Sin la capa de enlace de datos, un protocolo de capa de red, tal como IP, tendria que tomar medidas para conectarse con todos los tipos de medios que pudieran existir a lo largo de la ruta de envio.

el estandar IEEE separa la capa de enlace de datos en dos capas de LLC- Control de enlace logico y Control de acceso al medio

El modelo OSI separa la capa ethernet en la capa fisica y en la capa de enlace de datos

### Subcapa LLC

Se comunica entre el software de red en las capas superiores y el hardware del dispositivo en capapas inferiores. Coloca en la trama la indormacion que identifica que protocolo de capa de red se utiliza para la trama

### Subcapa MAC




