# 1. ¿Qué es IPv6? ¿Por qué es necesaria su implementación?

IPv6 es la nueva versión del protocolo IP. El principal motivo del desarrollo de IPv6 es el agotamiento de direcciones de IPv4, puesto que el crecimiento de Internet fue demasiado grande y no se había previsto, por lo que el número de direcciones brindadas por IPv4 no era suficiente para el futuro de Internet.

Las direcciones IPv6 ahora tienen 128 bits. El formato de las direcciones IPv4 podía brindar hasta 2<sup>32</sup> direcciones distintas, mientas que el nuevo IPv6 tiene 2<sup>128</sup>.

La cabecera IPv6 tiene los siguientes campos:

| Nombre | Tamaño | Descripción |
| ------ | ------ | ----------- |
| Dirección origen | 128 bits | Dirección IPv6 de la que proviene el paquete. |
| Dirección destino | 128 bits | Dirección IPv6 a la que se dirige el paquete. |
| Versión IP | 4 bits | Indica en qué versión de IP está el paquete de red (4 para IPv4; 6 para IPv6). Tener este campo en 4 no garantiza la compatibilidad. |
| Clase de tráfico | 8 bits | Usado para dar prioridad a ciertos datagramas de un flujo, o a ciertas aplicaciones. |
| Etiqueta de flujo | 20 bits | Indica la calidad de servicio del paquete. Este es un servicio nuevo de IPv6, que sirve para determinar los paquetes que pertenecen a un flujo para el que el emisor requiere un tratamiento especial. |
| Longitud de datos | 16 bits | Contiene el tamaño en bytes de los datos que lleva el datagrama + los 40 bytes de la cabecera. |
| Cabecera siguiente | 8 bits | Le indica al protocolo de transporte en qué orden corresponden el paquete enviado en este datagrama. |
| Límite de saltos | 8 bits | Indica la cantidad de saltos que puede dar un datagrama antes de ser descatado. Cuando pasa por cada router, este campo se decrementa en 1; si llega a 0, se descarta. |

# 2. ¿Por qué no es necesario el campo Header Length en IPv6?

Porque el header tiene una longitud fija de 40 bytes, a diferencia de IPv4 que el header tenía varios campos opcionales.

# 3. ¿En qué se diferencia el checksum de IPv4 e IPv6? Y en cuánto a los campos checksum de TCP y UDP, ¿sufren alguna modificación en cuanto a su obligatoriedad de cálculo?

IPv6 no tiene un campo de checksum; la protección se considera garantizada por los protocolos de la capa de enlace y los de la capa de transporte.

En TCP el checksum ya era obligatorio, así que no hay modificación.

En UDP podía obviarse el checksum y dejar el campo en 0; ahora, en IPv6, su cálculo es obligatorio.

# 4. ¿Qué sucede con el campo Opciones en IPv6? ¿Existe, en IPv6, algún forma de enviar información opcional?

La cabecera IPv6 no tiene un campo de opciones; sin embargo, pueden agregarse a través del campo "Next Header". Este campo podría apuntar a una nueva cabecera que contiene las opciones adicionales que deseemos.

# 5. Si quisiese que IPv6 soporte una nueva funcionalidad, ¿cómo lo haría?

Hay que asignarla a través del Next Header de cada datagrama.

# 6. ¿Es necesario el protocolo ICMP en IPv6? ¿Cumple las mismas funciones que en IPv4?

Existe una nueva versión de ICMP para IPv6, llamada ICMPv6. A diferencia de IPv4, que ICMP podía no usarse, para la arquitectura de IPv6 es obligatorio que todos los nodos lo soporten.

Los mensajes de ICMPv6 son usados para reconocer errores, detectar direcciones a través de multicast, etc. Los mensajes se dividen en *mensajes de error* o *mensajes informativos*.