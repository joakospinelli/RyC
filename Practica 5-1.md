# 1) ¿Cuál es la función de la capa de transporte?

La capa de transporte es la encargada de transferir los datos entre el emisor y el receptor de una comunicación a través de la red, independientemente de la red física de cada uno.

Para esta capa se usan principalmente dos protocolos: TCP y UDP. TCP es un protocolo de comunicación más seguro y fiable, mientras que UDP es más rápido. Ambos protocolos complementan al protocolo IP en la capa de red.

# 2) Describa la estructura del segmento TCP y UDP

### TCP
El segmento TCP está formado por una cabecera y un espacio para los datos.

La cabecera TCP tiene 11 campos (+1 que está reservado para uso futuro):

| Nombre | Tamaño | Descripción |
| --- | --- | --- |
| Puerto origen | 16 bits | Indica el puerto emisor del cliente |
| Puerto destino | 16 bits | Indica el puerto receptor en el servidor |
| Nro. de secuencia | 32 bits | Identifica el flujo de datos enviado por el emisor TCP. Cuando se inicia la conexión y se activa el flag `SYN`, este campo almacena el ISN (Initial Sequence Number) |
| Nro. de reconocimiento | 32 bits | Contiene el siguiente número de secuencia que el emisor del segmento espera recibir. Se envía y valida a través del flag `ACK` |
| Longitud | 4 bits | Indica el tamaño total de la cabecera |
| Flags | 9 bits | Espacio reservado para las 9 flags de TCP |
| Tamaño de ventana | 16 bits | Usado para controlar el flujo de datos Indica el mayor número de bytes que el receptor está dispuesto a aceptar |
| Checksum | 16 bits | Verifica que no haya cambios en los datos desde la emisión hasta la recepción |
| Puntero urgente | 16 bits  | Indica la cantidad de bytes en la que terminan los datos urgentes. Es relativo al nro. de secuencia |
| Opciones | Opcional | Permite agregar características no definidas en las cabeceras |
| Relleno | Opcional | Las cabeceras TCP deben tener un tamaño en bits múltiplo de 32, por lo que esta cabecera agrega información aleatoria para llegar (si es necesario).

### UDP
El segmento UDP está formado por una cabecera y un espacio para los datos. El espacio para datos es de máximo 32 bits.

La cabecera UDP tiene 4 campos:

| Nombre | Tamaño | Descripción |
| --- | --- | --- |
| Puerto origen | 16 bits | (OPCIONAL) Indica el puerto emisor del cliente |
| Puerto destino | 16 bits | Indica el puerto emisor del servidor |
| Longitud  | 16 bits | Indica el tamaño total del datagrama, incluyendo los datos. El valor mínimo son 8 bytes. |
| Checksum | 16 bits | (OPCIONAL) Verifica que no haya cambios en los datos desde la emisión hasta la recepción |

# 3) ¿Cuál es el objetivo del uso de puertos en el modelo TCP/IP?

El uso de puertos es importante para el reconocimiento de los *sockets*. Estos son interfaces por las que los datos pasan de una red al proceso que los necesita. TCP/IP usa las direcciones IP de los clientes y servidores para establecer las comunicaciones, pero los programas emisores y receptores se identifican a través de los puerto origen-destino, respectivamente.

El uso de puertos permtie identificar a los sockets, para que las aplicaciones del cliente puedan comunicarse con las del servidor. Si las aplicaciones usan protocolos de aplicación conocidos, pueden usar puertos reservados (0-1023); si no, usarán puertos genéricos (1024+).

# 4) Compare TCP y UDP en cuanto a:

## a. Confiabilidad.

| UDP | TCP |
| --- | --- |
| Al igual que el protocolo IP, no es fiable. La comprobación de errores depende de las aplicaciones | Es fiable ya que usa técnicas de control de flujo, números de secuencia, temporizadores y mensajes de reconocimiento |
| No garantiza que los segmentos lleguen al proceso destino, tampoco que lleguen en orden o se conserve la integridad de los datos. | Garantiza que los datos transmitidos por el proceso emisor sean entregados al proceso receptor, correctamente y en orden. Continuará reenviando un segmento hasta que la recepción del mismo haya sido confirmada por el destino. |

### b. Multiplexación.

| UDP | TCP |
| --- | --- |
| Utiliza multiplexación y demultiplexación sin conexión. | Utiliza multiplexación y demultiplexación orientada a la conexión. |
| Identifica el socket destino mediante la dirección IP destino y el puerto destino. Además el segmento tiene una dirección de retorno, por si el receptor desea devolver un segmento al emisor. | Identifica el socket con una tupla formada por `IP Origen - IP Destino - Puerto Origen - Puerto Destino`. Dos segmentos TCP entrantes con direcciones IP de origen o números de puerto de origen diferentes (con la excepción de un segmento TCP que transporte la solicitud original de establecimiento de conexión) serán dirigidos a dos sockets distintos. |

### c. Orientado a la conexión.

| UDP | TCP |
| --- | --- |
| Es un protocolo sin conexión. No tiene lugar una fase de establecimiento de la conexión entre las entidades de la capa de transporte emisora y receptora previa al envío del segmento. | Lleva a cabo un proceso de establecimiento de la conexión en tres fases antes de iniciar la transferencia de datos. |
| No mantiene información del estado de la conexión. | Mantiene información acerca del estado de la conexión en los sistemas terminales. |
| | La conexión es punto a punto. Cada lado de la conexión tiene su propio buffer de emisión y su propio buffer de recepción. |


### d. Controles de congestión.

| UDP | TCP |
| --- | --- |
| No implementa control de congestión. | Dispone de un mecanismo de control de congestión que regula el flujo del emisor TCP de la capa de transporte cuando uno o más de los enlaces existentes entre los hosts de origen y de destino están excesivamente congestionados. |
| | El emisor dispone de una ventana de recepción. Se emplea para proporcionar al emisor una idea de cuánto espacio libre hay disponible en el buffer del receptor. |

### e. Utilización de puertos.

| UDP | TCP |
| --- | --- |
| El socket está definido por el número de puerto destino y la dirección IP del destino. | El socket está definido para cada extremo de la conexión por una tupla de 4 elementos:  `IP origen - Puerto origen - IP destino - Puerto destino` |
| Muchos clientes pueden enviar datos por un mismo socket. | Por cada socket pueden intercambiar datos únicamente dos procesos.|

# 5) La PDU de la capa de transporte es el segmento. Sin embargo, en algunos contextos suele utilizarse el término datagrama. Indique cuándo.

Según los RFC, el nombre de los paquetes de la capa de transportes es "segmentos", mientras que el término "datagrama" se usa para los paquetes en la capa de red.

Sin embargo, a veces se usa el término "datagrama" para referirse a los paquetes transmitidos por el protocolo UDP (incluso en los mismos RFC).

# 6) Describa el saludo de tres vías de TCP. ¿Se utiliza algo similar en UDP?

El saludo en tres vías es un mecanismo que usa TCP para establecer las conexiones.

* **Paso 1**: el cliente envía un segmento al servidor sin datos de la aplicación, pero con la flag `SYN` en 1 y un número de secuencia generado aleatoriamente. Este segmento se llama "Segmento SYN".
* **Paso 2**: cuando el servidor recibe el segmento SYN, asigna los buffers y variables TCP para atender esta conexión. Luego le devuelve un segmento TCP al cliente, con la siguiente información en la cabecera:
  * Flag `SYN`en 1.
  * El campo de reconocimiento toma el valor del número de secuencia del cliente (NSI Cliente) + 1.
  * El servidor genera su propio número de secuencia (NSI Servidor) y lo guarda en el campo del número de secuencia del segmento. El NSI del cliente y del servidor se van a utilizar para validar los orígenes de los segmentos.

  Este segmento se llama "Segmento SYN/ACK".
* **Paso 3**: cuando el cliente recibe el segmento SYN/ACK, también asigna los buffers y variables TCP para continuar con la conexión, y envía un último segmento con la siguiente información:
  * Flag `SYN` en 0, indicando que ya se estableció la conexión.
  * En el campo de reconocimiento se almacena el valor `NSI Servidor + 1`, para confirmar el segmento de conexión con el servidor.
  * En este segmento ya se pueden transportes los datos de la aplicación del cliente al servidor.

Una vez que terminaron el saludo de tres vías, el cliente y el servidor pueden enviarse segmentos con datos entre sí hasta que terminen la conexión; todos estos segmentos van a tener el flag `SYN` en 0.

En UDP no hace falta usar este mecanismo ni similares, puesto que ese protocolo no establece conexiones entre procesos.

# 7) Investigue qué es el ISN (Initial Sequence Number). Relaciónelo con el saludo de tres vías.

El número de secuencia inicial (ISN) es un campo de 32 bits asignado al establecer cada conexión de TCP. A cada bit transmitido en TCP se le asigna un ISN aleatorio y único.

Este número permite identificar a los segmentos del cliente y del servidor, además de que permite controlar el origen y el orden en el que se reciben.

# 8) Investigue qué es el MSS. ¿Cuándo y cómo se negocia?

El tamaño máximo del segmento (Max segment Size, MSS) es una configuración opcional en la cabecera TCP. Permite acordar el tamaño máximo que pueden tener los segmentos transferidos en una conexión sin ser fragmentados.

En una conexión, el MSS óptimo es aquel que es menor al MTU (Unidad máxima de transferencia), que indica la unidad de datos más grande que puede transmitir el protocolo de la capa de enlace. El MTU se calcularía como: `MSS + Cabeceras IP + Cabeceras TCP/UDP`.

Cuando se intenta enviar un archivo más grande que el MSS, el archivo se va a enviar fragmentado, en fragmentos con tamaño igual al MSS.

# 9) Utilice el comando `ss` para obtener la siguiente información de su PC:

## a. Para listar las comunicaciones TCP establecidas.

Con la opción `-t`.

<img src="./screenshots/Practica 5-1/ej9a.png">

## b. Para listar las comunicaciones UDP establecidas.

Con la opción `-u`.

<img src="./screenshots/Practica 5-1/ej9b.png">

## c. Obtener sólo los servicios TCP que están esperando comunicaciones

Con la opción `-l`. Están todas en estado "LISTEN".

<img src="./screenshots/Practica 5-1/ej9c.png">

## d. Obtener sólo los servicios UDP que están esperando comunicaciones

Con la opción `-l`. Están todas en estado "UNCONN".

<img src="./screenshots/Practica 5-1/ej9d.png">

## e. Repetir los anteriores para visualizar el proceso del sistema asociado a la conexión.

Con la opción `-p`.

<img src="./screenshots/Practica 5-1/ej9e-1.png">

<img src="./screenshots/Practica 5-1/ej9e-2.png">

## f. Obtenga la misma información planteada en los items anteriores usando el comando `netstat`.

a. `netstat -t -p`

b. `netstat -u -p`

c. `netstat -t -l -p`

d. `netstat -u -l -p`

# 10) ¿Qué sucede si llega un segmento TCP con el flag SYN activo a un host que no tiene ningún proceso esperando en el puerto destino de dicho segmento (es decir, que dicho puerto no está en estado LISTEN)?

Si se envía un segmento TCP a un puerto que no está esperando comunicaciones, éste va a responder con un segmento especial indicando el reinicio de la operación; esto se indica con la flag `RST` en 1. Ese segmento también se llama "Segmento RST/ACK".

## a. Utilice `hping3` para enviar paquetes TCP al puerto destino 22 de la máquina virtual con el flag SYN activado.

Con la opción `-p` establezco el número de puerto destino. Con la opción `-S` activo el flag SYN.

<img src="./screenshots/Practica 5-1/ej10a.png">

Al terminar el comando nos muestra las estadísticas, con la cantidad de paquetes transmitidos/recibidos, la pérdida de paquete y el promedio de la velocidad de transmisión.

## b. Utilice `hping3` para enviar paquetes TCP al puerto destino 40 de la máquina virtual con el flag SYN activado.

<img src="./screenshots/Practica 5-1/ej10b.png">

## c. ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores? ¿Puede explicar a qué se debe?

La diferencia principal es que el `hping` al puerto 22 responde con el flag `SA`, mientras que al puerto 40 responde con el flag `RA`.

El flag `SA` indica que se respondió con un segmento SYN/ACK (en el saludo de tres vías), por lo que la conexión fue aceptada y el puerto destino está abierto.

El flag `RA` indica que se respondió con un segmento RST/ACK, por lo que el puerto destino no está abierto para recibir comunicaciones.

Con el comando `ss -l -n -t` podemos ver los puertos que están esperando comunicaciones TCP. En la lista aparece el puerto 22, pero no el puerto 40.

<img src="./screenshots/Practica 5-1/ej10c.png">

(El puerto 22 corresponde al servicio SSH. Con la opción `-n` podemos hacer que nos muestre los números de puerto en vez de los nombres de los servicios)

# 11) ¿Qué sucede si llega un datagrama UDP a un host que no tiene a ningún proceso esperando en el puerto destino de dicho datagrama (es decir, que dicho puerto no está en estado LISTEN)?

Si se envía un segmento (datagrama?? 🤨) UDP a un puerto que no está esperando comunicaciones, éste va a responder con un paquete ICMP especial para indicar que no está esperando comunicaciones.

## a. Utilice `hping3` para enviar datagramas UDP al puerto destino 5353 de la máquina virtual.

Con la opción `-2` indico que quiero enviar un datagrama UDP.

<img src="./screenshots/Practica 5-1/ej11a.png">

## b. Utilice `hping3` para enviar datagramas UDP al puerto destino 40 de la máquina virtual.

<img src="./screenshots/Practica 5-1/ej11b.png">

## c. ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores? ¿Puede explicar a qué se debe?

El envío de datagramas al puerto 5353 se envía correctamente, pero no recibimos ninguna respuesta en consola porque en el protocolo UDP el receptor no le confirma al emisor si recibe o no los datos.

Cuando enviamos datos al puerto 40, en consola recibimos un paquete ICMP, que indica que no se pudo establecer la comunicación con el puerto (*"Port Unreachable"*).

Con el comando `ss -uln` podemos ver los puertos que están esperando comunicaciones TCP. En la lista aparece el puerto 5353, pero no el puerto 40.

<img src="./screenshots/Practica 5-1/ej11c.png">

# 12) Investigue los distintos tipos de estado que puede tener una conexión TCP

| Nombre | Descripción |
| --- | --- |
| **LISTEN** | Está esperando una solicitud TCP. |
| **SYN-SENT** | Se envió una petición de conexión TCP y se está esperando recibir una petición que coincida con la enviada. |
| **SYN-RECEIVED** | Se envió y recibió una petición de conexión TCP, y se está esperando que se confirme la conexión. |
| **ESTABLISHED** | Se estableció la conexión TCP. El cliente TCP puede enviar y recibir segmentos con carga útil de datos. |
| **FIN-WAIT-1** | El cliente envió una petición para cerrar la conexión TCP, y se está esperando que se reciba una petición TCP del servidor reconociendo la petición enviada. |
| **FIN-WAIT-2** | El cliente espera a recibir una petición del servidor indicando el cierre de la conexión. |
| **CLOSE-WAIT** | Indica que el host remoto quiere cerrar la conexión, y le envió un mensaje `ACK` al cliente para que envíe los datos restantes o acepte el cierre. |
| **CLOSING** | Estado especial en el que ambos lados de la conexión TCP intentaron cerrar la conexión al mismo tiempo. Esto sucede cuando ambos envian un mensaje `FIN`. |
| **LAST-ACK** | Se espera el último segmento de reconocimiento para cerrar la conexión TCP. |
| **TIME-WAIT** | El cliente espera un tiempo para asegurarse de que el servidor TCP recibió el reconocimiento de la terminación de la conexión. |
| **CLOSED** | No existe conexión. |

# 13) Use CORE para armar una topología como la siguiente, sobre la cual deberá realizar:

## a. En ambos equipos inspeccionar el estado de las conexiones y mantener abiertas ambas ventanas con el comando corriendo para poder visualizar los cambios a medida que se realiza el ejercicio.

Estado inicial de `watch -n1 'ss -nat'`:

<img src="./screenshots/Practica 5-1/ej13a.png">

## b. En "Servidor", utilice la herramienta *ncat* para levantar un servicio que escuche en el puerto 8001/TCP. Utilice la opcion `-k` para que el servicio sea persistente. Verifique el estado de las conexiones.

<img src="./screenshots/Practica 5-1/ej13b.png">

## c. Desde "Cliente" conectarse a dicho servicio utilizando también la herramienta *ncat*. Inspeccione el estado de las conexiones.

<img src="./screenshots/Practica 5-1/ej13c.png">

## d. Iniciar otra conexión desde "Cliente" de la misma manera que la anterior y verificar el estado de las conexiones. ¿De qué manera puede identificar cada conexión?

<img src="./screenshots/Practica 5-1/ej13d.png">

Podemos identificar las conexiones porque en el cliente están en puertos distintos (una en el puerto 59912 y otra en el 59940).

Esto lo podemos ver en el comando `ss -nat` tanto del cliente como del servidor:
* En el cliente, en la columna de `Local Address:Port`.
* En el servidor, en la columna de `Peer Address:Port`.

## e. En base a lo observado en el item anterior, ¿es posible iniciar más de una conexión desde el cliente al servidor en el mismo puerto destino? ¿Por qué? ¿Cómo se garantiza que los datos de una conexión no se mezclarán con los de la otra?

Es posible, puesto que desde el cliente todas las conexiones van a realizarse desde un puerto distinto.

Para garantizar que no se muestren los datos, el servidor tiene guardado el puerto de origen desde el cual el cliente realizó la comunicación; cuando tenga que enviar datos por esa comunicación en concreto, va a hacerlo hacia ese mismo puerto.

## f. Analice en el tráfico de red, los flags de los segmentos TCP que ocurren cuando:

#### i. Cierra la última conexión establecida desde "Cliente". Evalúe los estados de las conexiones en ambos equipos

<img src="./screenshots/Practica 5-1/ej13f-1.png">

Primero la conexión del port 59940 estuvo en estado `TIME-WAIT` por un tiempo, y después desapareció tanto del cliente como del servidor.

En el Wireshark me mandó como 920 millones de segmentos TCP así que no los voy a revisar todos

#### ii. Corta el servicio de *ncat* en el Servidor (Ctrl+C). Evalúe los estados de las conexiones en ambos equipos.

<img src="./screenshots/Practica 5-1/ej13f-2.png">

Hicimos que el Servidor deje de aceptar las conexiones en el puerto 8001, pero todavía dejamos la conexión que mandamos desde *Cliente*, así que va a quedar en estado `CLOSE-WAIT` hasta que la cerremos.

Después de un tiempo, la conexión del Servidor que estaba en `FIN-WAIT-2` desapareció.

#### iii. Cierra la conexión en el cliente. Evalúe nuevamente los estados de las conexiones.

<img src="./screenshots/Practica 5-1/ej13f-3.png">

El estado `LAST-ACK` en *Cliente* indica que cerró la conexión, pero que todavía el puerto sigue abierto esperando un último mensaje `ACK` desde *Servidor*.

Sin embargo, como cerramos el puerto de *Servidor* antes de terminar la conexión de *Cliente*, se quedó en ese estado esperando a recibir ese último mensaje `ACK`. Eventualmente finalizó por su cuenta y desapareció de la lista.

# 14) Dada la siguiente salida del comando `ss`, responda:

<img src="./screenshots/Practica 5-1/ej14.png">

## a. ¿Cuántas conexiones hay establecidas?

Hay 9 conexiones TCP en estado `ESTAB`, por lo que están establecidas (No sé cómo son los estados de UDP)

## b. ¿Cuántos puertos hay abiertos a la espera de posibles nuevas conexiones?

Hay 3 conexiones TCP en estado `LISTEN` (También hay 2 conexiones UDP en ese estado pero el estado de espera de UDP era `UNCONN` 🤨).

Los puertos son 22, 80 y 25.

## c. El cliente y el servidor de las comunicaciones HTTPS (puerto 443), ¿residen en la misma máquina?

Hay 9 conexiones TCP hacia el puerto 443 en el servidor. En todas las conexiones la dirección local es distinta a la dirección remota.

## d. El cliente y el servidor de la comunicación SSH (puerto 22), ¿residen en la misma máquina?

Sí 👍. Tienen la misma dirección IP y en el apartado `users` vemos que ambos tienen el mismo PID, por lo que probablemente vengan de la misma aplicación.

## e. Liste los nombres de todos los procesos asociados con cada comunicación. Indique para cada uno si se trata de un proceso cliente o uno servidor.

* `SSHD`
* `Apache2`
* `Named`
* `X-WWW-Browser`
* `Postfix`
* `SSH`

## f. ¿Cuáles conexiones tuvieron el cierre iniciado por el host local y cuáles por el remoto?

Las conexiones en estado `TIME-WAIT` fueron cerradas por el host local. Las que están en estado `CLOSE-WAIT` fueron cerradas por el host remoto.

## g. ¿Cuántas conexiones están aún pendientes por establecerse?

Una sola, la que está en estado `SYN-SENT`.

# 15) Dadas las salidas de los siguientes comandos ejecutados en el cliente y el servidor, responder:

<img src="./screenshots/Practica 5-1/ej15.png">

## a. ¿Qué segmentos llegaron y cuáles se están perdiendo en la red?

Las columnas que están entre el estado y la dirección IP local son `Recv-Q` y `Send-Q`, que indican lo siguiente:

* `Recv-Q` indica los bytes que se recibieron pero que el programa asociado al socket todavía no copió.
* `Send-Q` indica los bytes enviados que no fueron reconocidos por el host remoto.

En este caso, el segmento en estado `SYN-SENT` es el que se está perdiendo.

## b. ¿A qué protocolo de capa de aplicación y de transporte se está intentando conectar el cliente?

El cliente está intentando conectarse al puerto 110, que es un puerto reservado para el protocolo `POP3`.

## c. ¿Qué flags tendría seteado el segmento perdido?

Tenía seteados los flags `SYN` y `ACK`.