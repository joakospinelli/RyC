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

# 7) 


