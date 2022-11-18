# 1. ¿Qué función cumple la capa de enlace? Indique qué servicios presta esta capa.

La capa de enlace es la encargada de transmitir la información directamente entre dos dispositivos conectados. El PDU de esta capa es la "trama".

A diferencia de la capa de red, que se encarga de mover los datos entre un host y otro, la capa de enlace se encarga de hacerlo de nodo a nodo; este transporte es mucho más simple y directo, puesto que para funcionar los nodos **deben** estar conectados entre sí.

Algunos de los servicios que brinda son:
* **Entramado:** casi todos los protocolos de la capa de enlace encapsulan un datagrama de la capa de red dentro de una trama de transmitirla a través del enlace. La estructura de la trama está especificada por el protocolo de la capa de enlace.
* **Acceso al enlace:** las reglas para la transmisión y el acceso a los enlaces se definen a través de los protocolos de control de acceso al medio (MAC).
* **Entrega fiable:** los protocolos de esta capa garantizan que va a transportar cada datagrama de la capa de red a través del enlace sin que se produzcan errores. La entrega fiable en la capa de enlace puede considerarse una sobrecarga innecesaria en aquellos enlaces que tengan una baja tasa de errores de bit. Por esta razón, muchos protocolos de la capa de enlace para enlaces cableados no proporcionan un servicio de entrega fiable
* **Control de flujo:** los nodos situados en cada extremo de un enlace tienen una capacidad limitada de almacenamiento en buffer de las tramas. El protocolo de la capa de enlace puede proporcionar un mecanismo de control de flujo para evitar que el nodo emisor al otro lado del enlace abrume al nodo receptor situado en el otro extremo.
* **Detección de errores:** podrían llegar a producirse errores de bit debido a la atenuación de las señales y al ruido electromagnético. Puesto que no existe ninguna necesidad de reenviar un datagrama que contenga un error, muchos protocolos de la capa de enlace proporcionan un mecanismo para detectar dichos errores de bit. La detección de errores en la capa de enlace normalmente es más sofisticada y se implementa en Hardware.
* **Corrección de errores:** La corrección de errores es similar a la detección de errores, salvo porque el receptor no sólo detecta si hay bits erróneos en la trama, sino que también determina exactamente en qué puntos de la trama se han producido los errores (y luego corrige esos errores).
* **Semiduplex y full-duplex:** con la transmisión full-duplex, los nodos de ambos extremos de un enlace pueden transmitir paquetes al mismo tiempo. Sin embargo, con la transmisión semiduplex un mismo nodo no puede transmitir y recibir al mismo tiempo. 

# 2. Compare los servicios de la capa de enlace con los de la capa de transporte.

La capa de enlace tiene servicios similares a los de la capa de transporte; por ejemplo, el servicio de entrega fiable (presente en TCP), el control de flujo y la detección de errores.

Sin embargo, la mayor diferencia entre estos servicios es que la capa de transporte opera entre hosts terminales, mientras que la capa de enlace opera entre nodos. Los resultados de los servicios de la capa de transporte son más inmediatos, puesto que se realizan durante la transmisión de la capa de red entre los nodos adyacentes.

# 3. Direccionamiento Ethernet:
## ¿Cómo se identifican dos máquinas en una red Ethernet?

A través de una dirección MAC.

## ¿Cómo se llaman y qué características poseen estas direcciones?

La dirección MAC está formada por 6 bloques de 8 bits y son únicas para cada dispositivo.

## ¿Cuál es la dirección de broadcast en capa de enlace? ¿Qué función cumple?

La dirección de broadcast es la `FF:FF:FF:FF:FF:FF`. Las tramas dirigidas a esta dirección son enviadas a todos los dispositivos conectados a una red local; por lo general, los datagramas de red dirigidos a la dirección de Broadcast de la red son enviados a esta dirección en la capa de enlace.

También se usa en el protocolo ARP para obtener la dirección MAC correspondiente a una dirección IP *(pero este protocolo hay que explicarlo en otra pregunta más adelante 😎)*.

# 4. Sobre los dispositivos de capa de enlace:
## Enumere dispositivos de capa de enlace y explique sus diferencias.

* `Hub`: dispositivo de la capa física. Actúa como un repetidor que recibe un bit y lo retransmite a los demás dispositivos conectados; las redes basadas en Hubs son también redes de broadcast, porque se envían copias a todas las interfaces. Cuando un Hub recibe tramas de dos o más dispositivos a la vez, se produce una colisión y ambos deben retransmitirla.
* `Switch`: dispositivo que reemplazó al Hub. Todos los dispositivos de la red se conectan al Switch, y el Switch realiza buffering entre sus hosts. Tiene una tabla de dispositivos con la cual sabe a dónde tiene que retransmitir cada trama; de esta manera, no tiene que hacer siempre broadcast como el Hub (salvo que no conozca al dispositivo). No tiene colisiones.

## ¿Qué es una colisión?

Una colisión se produce cuando llegan dos datos en simultáneo, y hay riesgo de que la información se mezcle.

## ¿Qué dispositivos dividen dominios de broadcast?

Los dominios de broadcast están formados por los dispositivos que van a recibir un mensaje cuando se envía una trama a la dirección de Broadcast.

En Ethernet, el dominio de Broadcast se dividen por los routers.

## ¿Qué dispositivos dividen dominios de colisión?

Todos los entornos de los medios compartidos, como las estructuras de estrella con Hubs, son dominios de colisión.

Cuando un host se conecta a un puerto de Switch, se crea una conexión dedicada. Esta conexión se considera como un dominio de colisiones individual, dado que el tráfico se mantiene separado de cualquier otro; gracias a esto, se eliminan las posibilidades de colisión.

# 5. Describa el algoritmo de acceso al medio en Ethernet. ¿Es orientado a la conexión?

Ethernet es un servicio sin conexión y no es fiable.

Los pasos del algoritmo de acceso al medio son:

1. El adaptador obtiene un datagrama de la capa de red, prepara una trama Ethernet y la coloca en un buffer del adaptador.
2. Si el adaptador detecta que el canal está inactivo, comienza a transmitir la trama. Si el adaptador detecta que el canal está ocupado, espera hasta comprobar que no haya intensidad de señal y luego comienza a transmitir la trama.
3. Mientras está transmitiendo, el adaptador monitoriza la presencia de señales procedentes de otros adaptadores. Si el adaptador transmite la trama completa sin detectar ninguna señal procedente de otros adaptadores, concluye que ha terminado su trabajo con esa trama.
4. Si el adaptador detecta intensidad de señal procedente de otros adaptadores mientras está transmitiendo, deja de transmitir su trama y transmite una señal de interferencia.
5. Después de abortar la transmisión de la trama (es decir, de transmitir la señal de interferencia), el adaptador entra en la fase de espera exponencial. Específicamente, a la hora de transmitir una determinada trama, el adaptador selecciona un valor aleatorio para K del conjunto {0,1,2, . . . , 2<sup>m–1</sup>}, donde m = min(n,10). El adaptador espera entonces K 512 periodos de bit y vuelve al Paso 2.

# 6. ¿Cuál es la finalidad del protocolo ARP?

El protocolo ARP es el encargado de traducir las direcciones IP de la capa de red a direcciones MAC de la capa de enlace. Un nodo sólo puede usar ARP para resolver las direcciones IP dentro de su propia subred.

Esta traducción se realiza a través de una tabla ARP. Cada entrada de esta tabla contiene:

* `Dirección IP:` dirección del nodo en la capa de red.
* `Dirección MAC:` dirección del nodo en la capa de enlace.
* `TTL:` tiempo que indica cuándo la entrada deja de ser válida y será eliminada.

Si un nodo quiere enviarle un datagrama a otro y no lo tiene en su tabla, entonces debe enviar un **paquete ARP** al adaptador; éste lo recibe, lo reenvía a la dirección de Broadcast y espera a que el nodo con la dirección IP que buscamos le conteste. Cuando obtiene la respuesta, le devuelve la dirección IP encontrada y el emisor puede actualizar su tabla ARP para enviar el mensaje al destino.

# 7. Dado el siguiente esquema de red, responda:

<img src="./screenshots/Practica 10/ej7.png">

## a. Suponiendo que las tablas de los switches están llenas con la información correcta, responda quién escucha el mensaje si:

### i. La estación 1 envía una trama al servidor 1.

Estación 1 va a enviar la trama al Hub. Debido a que los Hubs funcionan reenviando las tramas mediante Broadcast, todos los dispositivos conectados a él van a recibir la trama (menos al emisor); en este caso:

* Estación 2
* Estación 3
* Servidor 1
* Estación 4
* Estación 5
* Switch 1

### ii. La estación 1 envía una trama a la estación 11.

Estación 1 envía la trama al Hub y lo reenvía a todos los conectados a él. Switch 1 recibe el mensaje y lo retransmite únicamente al receptor; en este caso, Estación 11.

* Estación 2
* Estación 3
* Servidor 1
* Estación 4
* Estación 5
* Switch 1
* Estación 11

### iii. La estación 1 envía una trama a la estación 9.

Estación 1 envía la trama al Hub y lo reenvía a todos los conectados a él. Switch 1 recibe el mensaje y lo retransmite al Switch 2. Switch 2 lo retransmite a su Hub, por lo que el mensaje se reenvía al Broadcast.

* Estación 2
* Estación 3
* Servidor 1
* Estación 4
* Estación 5
* Switch 1
* Switch 2
* Estación 10
* Estación 9
* Estación 8

### iv. La estación 4 envía una trama a la MAC de broadcast.

Como todos los nodos van a recibir un mensaje de broadcast, lo van a difundir hacia todos los demás, incluso hacia los que no están conectados a la Estación 4. Como los routers limitan el dominio de Broadcast, el mensaje sólo se va a difundir dentro de esta subred.

### v. La estación 6 envía una trama a la estación 7.

A diferencia de los Hubs, los switches retransmiten el mensaje directamente al destino.

* Switch 2
* Estación 7

### vi. La estación 6 envía una trama a la estación 10.

Desde la Estación 6 se envía el mensaje al Switch 2, pero entre el Switch y el destino está el Hub como intermediario, que cuando recibe la trama la retransmite al Broadcast.

* Switch 2
* Estación 10
* Estación 9
* Estación 8

## b. ¿En qué situaciones se podrían producir colisiones?

Los switches no pueden producir colisiones, por lo que los únicos casos posibles de colisión son aquellos en los que hay un Hub como intermediario de la transmisión.

En los ejercicios anteriores, sería posible en `i, ii, iii, iv, vi`.