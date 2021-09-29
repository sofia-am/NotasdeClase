# Capa 1

Está implementada integramente en hardware.
## metodos y comunicacion

serie y paralelo, el 1ero el primero es menos costoso y menos velocidad, el 2do es mas costoso pero tiene mas velocidad.

otra característica de la comunicacion en paralelo es que tiene que ser síncrona.

simplex -> A le manda a B
half-duplex -> A le manda a B, o B le manda a A
full-duplex -> A le manda a B y B le manda a A

## medios de transmisión guiados

un medio de transmision no guiado son wifi, telefonicas, etc, donde no le podés decir por donde ir.
medios guiados son cables.


### cable coaxial 

el estandar que lo regula es el que nos dice como van a estar codificados los bits adentro del cable coaxial.

**tip**: podemos deducir la distancia y la capacidad del estándar. Por ejemplo 10Base5, donde la capaciad es 10Mbps y la distancia es 500 mts 

### par trenzado UTP

Shielder Twisted Pair STP, se usa cuando hay interferencia  y radiacion electromagnetica, puede saltar un bit y tenemos que reenviar la información.

Unshielded Twister Pair UTP, mas comunmente utilizado

Según como armamos las puntitas del par trenzado se trata de cable cruzado o derecho. Vamos a usar cable cruzado para comunicar directamente dispositivos de la misma capa del stack TCP/IP. De lo contrario se usa cable derecho.

A menos que tengamos Auto-MDI, que es un modulo en nuestra interfaz de red que se encarga de, via software, ordenar los canales independientemente de la implementación de cable. 

### fibra optica

fibra monomodo y multimodo, la mayor diferencia reside no en la capacidad, sino en la distancia que puede recorrer cada una. monomodo es mucho mas cara que la multimodo.

la fibra multimodo utiliza leds para transmitir. para transmitir ese led se utiliza un core, que es un cristal (por donde va rebotando) que es mucho mas amplio. mientras la luz viaja a través del canal, va rebotando y liberando energía, por ende perdiendo potencia. esto nos limita a menores distancias.

la monomodo por otro lado utiliza láser y no rebota nunca en las paredes del core que entonces es mucho mas finito por ende no va a rebotar, no pierde potencia y puede admitir mas distancias sin perder informacion. 

inclusive es mas barato transmitir en leds que hacerlo en láser.

### transceivers:

son las interfaces de un equipo con terminales opticas que nos permiten transformar los impulsos electricos a pulsos de luz. 

dependiendo del caso de uso existen diferentes factores de forma de los transceivers.



 