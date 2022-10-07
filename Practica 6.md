# 1) ¿Cual es el puerto por defecto que se utiliza en los siguientes servicios? Investigue en qué lugar en Linux y en Windows está descripta la asociación utilizada por defecto para cada servicio.

* `Web`: 80
* `SSH`: 22
* `DNS`: 53
* `Web seguro`: 443
* `POP3`: 110
* `IMAP`: 143
* `SMTP`: 25

En Linux esta información se guarda en el archivo `/etc/services`. En este archivo se guarda una entrada para cada servicio de la capa de aplicación, que contiene el nombre, número de puerto, protocolo de capa de transporte y alias.

Windows tiene un archivo similar almacenado en `C:/Windows/System32/drivers/etc/services`.

# 2)  Investigue qué es multicast. ¿Sobre cuál de los protocolos de capa de transporte funciona? ¿Se podría adaptar para que funcione sobre el otro protocolo de capa de transporte? ¿Por qué?

El multicast es una función de la red que permite enviar información simultáneamente a varios nodos de una red. El emisor selecciona un "grupo de multicast", formado por los nodos interesados en recibir ese tráfico de red, y sólo va a transmitirle los datos a esos (a diferencia del Broadcast que transmite a toda la red). Se usa a través del protocolo UDP.

No se utiliza TCP porque en este protocolo el emisor requiere sincronizarse con el receptor en un handshake 1 a 1. El multicast tiene múltiples receptores, por lo que el emisor no puede realizar las sincronizaciones y no puede responder en caso de que se pierdan algunos paquetes durante la transferencia.

# 4) Investigue cómo funciona el protocolo de aplicación FTP teniendo en cuenta las diferencias en su funcionamiento cuando se utiliza el modo activo de cuando se utiliza el modo pasivo ¿En qué se diferencian estos tipos de comunicaciones del resto de los protocolos de aplicación vistos?

FTP es un protocolo en la capa de aplicación que permite la transferencia de archivos entre un cliente y un servidor. Normalmente utiliza los puertos 20 y 21 (o sólo 21 dependiendo el modo).

Las transferencias en este protocolo requieren dos conexiones (una de control y una de datos); es por esto que utiliza dos puertos, a diferencia de la mayoría de protocolos de la capa de aplicación que usan sólo 1. Es un protocolo pensado para transferir archivos a máxima velocidad, pero como consecuencia es bastante inseguro puesto que no está cifrado.

Tiene dos posibles maneras de implementarse:
* **Modo pasivo**: el cliente inicia la conexión de control a través del puerto 21 y el servidor le responde indicándole en qué puerto debe conectarse para realizar la conexión de datos. Luego el cliente se conecta al puerto que le enviaron para iniciar la conexión de datos.
* **Modo activo**: el cliente se inicia la conexión de control hacia el puerto 21 con el servidor enviándole un puerto aleatorio en el que va a realizar la conexión de datos. Luego, el servidor es el encargado de iniciar la conexión hacia el puerto que le envió el cliente (el servidor siempre usa el puerto 20 para la conexión de datos en modo activo).

  El problema de este modo es que el cliente debe aceptar cualquier conexión en el puerto aleatorio, lo cual puede ser que cualquier otro dispositivo se conecta con el nuestro si estamos en una red insegura.

# 4) Suponiendo Selective Repeat; tamaño de ventana 4 y sabiendo que E indica que el mensaje llegó con errores. Indique en el siguiente gráfico, la numeración de los ACK que el host B envía al Host A.

En Selective Repeat, el host A envía un conjunto de paquetes y el host B devuelve un ACK por cada paquete que recibe o un NACK (mensaje de error) si tuvo algún problema al recibirlo.

Cuando se recibe un ACK, el host A va a enviar más paquetes (siempre respetando la ventana) teniendo en cuenta los que todavía no fueron verificados.

Cuando se recibe un error, el host A va a reenviar ese paquete cuando termine de recibir las confirmaciones de los demás paquetes enviados.

| Enunciado | Solución |
| --- | --- |
| <img src="./screenshots/Practica 6/ej4-1.png">|<img src="./screenshots/Practica 6/ej4-2.png"> |

# 5) ¿Qué restricción existe sobre el tamaño de ventanas en el protocolo Selective Repeat?

El tamaño de la ventana siempre debe ser menor o igual a la mitad del espacio de números de secuencia. Esto se debe a que la ventana se implementa como un buffer circular, entonces si fuese más grande podría haber paquetes representados por la misma posición en el buffer, pero en vueltas distintas.

Al hacer la ventana de ese tamaño se asegura de que las posiciones en el buffer no se superpongan entre paquetes distintos.

# 6) De acuerdo a la captura TCP de la siguiente figura, indique los valores de los campos borroneados.

<img src="./screenshots/Practica 6/ej6.png">

1. `[SYN]`. Es el primer segmento del handshake en 3 pasos de TCP. Lo podemos suponer porque el siguiente segmento es una respuesta ACK.
2. `3933822137`. Número de secuencia del cliente. Lo podemos suponer porque en el encabezado *Ack* del segmento siguiente (que corresponde al servidor) está este mismo número, pero incrementado en 1.
3. `172.20.1.1`. IP origen. En este caso va a ser esa porque es el tercer paso del handshake, que es realizado por el mismo que lo empezó (el cliente).
4. `172.20.1.100` IP destino. En este caso va a ser esa porque el cliente le tiene que enviar el segmento ACK al servidor para terminar el handshake.
5. `41749`. Puerto origen. Este segmento es enviado por el cliente, por lo que el puerto origen tiene que corresponder con el primer segmento que envía.
6. `vce`. Puerto destino. Este segmento es enviado por el cliente, por lo que el puerto destino tiene que corresponder con el primer segmento que envía; también lo podemos ver porque el segmento anterior (enviado por el servidor) es enviado desde este puerto. (Lo que no sé es por qué se llama *"vce"*, pero abajo en la captura dice `Source port: vce (11111)`, así que capaz es un alias para ese puerto 😴)
7. `[ACK]`. Segmento final del handshake en 3 pasos de TCP. Lo podemos suponer porque el primer segmento fue un [SYN] y el segundo fue [SYN, ACK].
8. `3933822138`. Número de secuencia del cliente. Va a quedar así porque en el segmento anterior el servidor lo incrementó en 1. Lo podemos suponer viendo el segmento *Ack* del segmento [SYN, ACK], o viendo el segmento *Seq* del segmento [SYN] y sumarle 1.
9. `1047471502`. Número de Ack del cliente. Este mismo número está en el encabezado *Seq* del segmento anterior (que corresponde al servidor) pero restado en 1; cuando lo recibe el cliente lo incrementa en 1 y lo vuelve a enviar en el encabezado *Ack*.

# 7) Dada la sesión TCP de la figura, completar los valores marcados con un signo de interrogación.

| Enunciado | Solución |
| --- | --- |
| <img src="./screenshots/Practica 6/ej7-1.png"> | <img src="./screenshots/Practica 6/ej7-2.png">|

# 8)  ¿Qué es el RTT y cómo se calcula? Investigue la opción TCP timestamp y los campos TSval y TSecr.

El RRT (*Round-trip time*) es el tiempo que tarda un paquete en volver a su emisor tras haber pasado por el destino.

Los timestamps de TCP son los que permiten medir el RTT. Están formados por dos campos:
* `TSval`: contiene un valor de 4 bytes que se le asigna al iniciar la conexión y va a ir incrementando a lo largo de la conexión.
* `TSecr`: repite el valor de *TSval* en el otro lado de la conexión. Esto permite calcular el RTT compararando el valor de este campo con el valor inicial del *TSval*.

# 9) Para la captura dada, responder las siguientes preguntas.
## a. ¿Cuántos intentos de conexiones TCP hay?

Hay 6 intentos de realizar la conexión. Podemos identificarlos buscando los segmentos SYN del handshake en 3 vías, puesto que tiene que enviarse uno para iniciar una conexión.

En Wireshark, podemos buscar estos segmentos filtrando por `tcp.flags.syn eq 1 and tcp.flags.ack eq 0`, para no contar los segmentos SYN/ACK que además del flag SYN tienen el ACK en 1.

<img src="./screenshots/Practica 6/ej9a.png">

## b. ¿Cuáles son la fuente y el destino (IP:port) para c/u?

| Origen | Destino |
| --- | --- |
| 10.0.2.10:46907 | 10.0.4.10:5001 |
| 10.0.2.10:45670 | 10.0.4.10:7002 |
| 10.0.2.10:45671 | 10.0.4.10:7002 |
| 10.0.2.10:46910 | 10.0.4.10:5001 |
| 10.0.2.10:54424 | 10.0.4.10:9000 |
| 10.0.2.10:54425 | 10.0.4.10:9000 |

## c. ¿Cuántas conexiones TCP exitosas hay en la captura? Cómo diferencia las exitosas de las que no lo son? ¿Cuáles flags encuentra en cada una?

Hay 4 conexiones exitosas. Las exitosas se diferencian de las fallidas porque se recibe el segmento SYN/ACK del handshake de 3 vías; las conexiones fallidas envían un segmento RST.

En Wireshark, podemos buscar estos segmentos filtrando por `tcp.flags.syn eq 1 and tcp.flags.ack eq 1`.

## d. Dada la primera conexión exitosa responder:

### i. ¿Quién inicia la conexión?

`10.0.2.10:46907`

### ii. ¿Quién es el servidor y quién el cliente?

El cliente es el que inicia la conexión (`10.0.2.10:46907`). El servidor es el host destino (`10.0.4.10:5001`).

### iii. ¿En qué segmentos se ve el 3-way handshake?

<img src="./screenshots/Practica 6/ej9d-1.png">

* Segmento SYN enviado por el cliente (10.0.2.10)
* Segmento SYN/ACK enviado por el servidor (10.0.4.10)
* Segmento ACK enviado por el cliente

### iv. ¿Cuáles ISNs se intercambian?

El ISN del cliente es `2218428254`.

El ISN del servidor es `1292618479`.

### v. ¿Cuál MSS se negoció?

En el segmento SYN enviado por el cliente, este solicita un MSS de `1460`.

### vi. ¿Cuál de los dos hosts enva la mayor cantidad de datos (IP:port)?

Con la opción de `Follow TCP Stream` vemos que el cliente (10.0.4.10) es el que envía más paquetes.

## e. Identificar primer segmento de datos (origen, destino, tiempo, número de fila y número de secuencia TCP).

El primer segmento de datos es el primer segmento con flag `[PSH, ACK]` después del handshake.

<img src="./screenshots/Practica 6/ej9e.png">

* `Origen`: 10.0.2.10:46907
* `Destino`: 10.0.4.10:5001
* `Tiempo`: 0.151826
* `Nro. de fila`: ??????????????
* `Nro. de secuencia`: 2218428255

### i. ¿Cuántos datos lleva?

En el campo de `Len` vemos que lleva 24 bytes.

### ii. ¿Cuándo es confirmado (tiempo, número de fila y número de secuencia TCP)?
Se confirma en el segmento con flag `ACK` después del segmento `PSH,ACK` (está arriba)

* `Origen`: 10.0.4.10:5001
* `Destino`: 10.0.2.10:46907
* `Tiempo`: 0.151925
* `Nro. de fila`: ??????????????
* `Nro. de secuencia`: 1292618480

### iii. La confirmación, ¿qué cantidad de bytes confirma?

???????????????

## f. ¿Quién inicia el cierre de la conexión? ¿Qué flags se utilizan? ¿En cuáles segmentos se ve (tiempo, número de fila y número de secuencia TCP)?

El cierre de la conexión lo inicia el cliente (10.0.2.10) con los flags `[FIN,PSH,ACK]`. Luego el servidor (10.0.4.10) responde con los flags `FIN,ACK` y finalmente el cliente cierra la conexión con un `ACK`.

<img src="./screenshots/Practica 6/ej9f.png">

# 10) Responda las siguientes preguntas respecto del mecanismo de control de flujo.
## a. ¿Quién lo activa? ¿De qué forma lo hace?

El control de flujo es activado por el cliente, indicándole al servidor la cantidad de datos que es capaz de aceptar en su buffer. Para esto se usa el campo `Window` del encabezado TCP.

## b. ¿Qué problema resuelve?

Esto permite controlar el tráfico de datos para que no se envíen más datos de los que el cliente puede procesar antes de recibir nuevos.

## c. ¿Cuánto tiempo dura activo y qué situación lo desactiva?

🐱‍🐉

# 11) Responda las siguientes preguntas respecto del mecanismo de control de congestión.
## a. ¿Quién lo activa el mecanismo de control de congestión? ¿Cuáles son los posibles disparadores?

El mecanismo de control de congestión es activado por el dispositivo receptor cuando determina que la red por la que se están comunicando está congestionada. Esto principalmente se debe a la pérdida de paquetes en la red.

## b. ¿Qué problema resuelve?

El control de congestión regula los paquetes enviados en una comunicación TCP para que haya menos tráfico en una red. Esto permite que haya una circulación de paquetes más tranquila y se pierdan menos.

Este problema se resuelve modificando el tamaño de la ventana de congestión para que el servidor envíe menos datos en cada comunicación.

## c. Diferencie "slow start" de "congestion-avoidance".

Slow-start es un algoritmo para controlar la congestión en TCP. Consiste en comenzar las transmisiones enviando un volumen de datos pequeño, e ir incrementándolo hasta que se note que la red está saturada; una vez que se satura la red, se regula la ventana para enviar una cantidad aceptable.

*(El congestion-avoidance no sé xd)*
