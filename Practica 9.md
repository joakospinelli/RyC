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

# 7. Transforme las siguientes direcciones MACs en Identificadores de Interfaces de 64 bits.

Para convertir la direccion MAC a un ID de interfaz:

* `Paso 1:` pasar el primer octeto de hexadecimal a binario.
* `Paso 2:` invertir el séptimo bit del primer octeto.
* `Paso 3:` pasar el primer octeto de nuevo a hexacedimal.
* `Paso 4:` agregar `ff:fe` en el medio (entre el tercer y cuarto octeto).
* `Paso 5:` agregar `fe80::` al principio de la dirección.
* `Paso 6:` pasar el primer octeto a hexadecimal.
* `Paso 7:` unificar los octetos con el de al lado para formar los campos de 16 bits.

## 00:1b:77:b1:49:a1

1. 0000 0000:1b:77:b1:49:a1

2. 0000 0010:1b:77:b1:49:a1

3. 2:1b:77:b1:49:a1

4. 2:1b:77:ff:fe:b1:49:a1

5. fe80::2:1b:77:ff:fe:b1:49:a1

5. fe80::2:1b:77:ff:fe:b1:49:a1 *// No cambia porque el 2 es igual*

6. fe80::21b:77ff:feb1:49a1

## e8:1c:23:a3:21:f4

1. 1110 1000:1c:23:a3:21:f4

2. 1110 1010:1c:23:a3:21:f4

3. 234:1c:23:a3:21:f4

4. 234:1c:23:ff:fe:a3:21:f4

5. fe80::234:1c:23:ff:fe:a3:21:f4

6. fe80::ea:1c:23:ff:fe:a3:21:f4

7. fe80::ea1c:23ff:fea3:21f4

# 8. ¿Cuál de las siguientes direcciones IPv6 no son válidas?

* `2001:0:1019:afde::1`: válida.

* `2001::1871::4`: inválida ( *::* sólo puede usarse una vez).

* `3ffg:8712:0:1:0000:aede:aaaa:1211`: inválida ( *"g"* no forma parte del sistema hexadecimal).

* `3::1`: válida.

* `::`: válida.

* `2001::`: válida.

* `3ffe:1080:1212:56ed:75da:43ff:fe90:affe`: válida.

* `3ffe:1080:1212:56ed:75da:43ff:fe90:affe:1001:`: inválida (contiene un grupo de más).

# 9. ¿Cuál sería una abreviatura correcta de 3f80:0000:0000:0a00:0000:0000:0000:0845?

* `3f80::a00::845`: no es correcta porque usa *::* dos veces.

* `3f80::a:845`: no es correcta porque ignora los bits en 0 entre *2a"* y *"845"*.

* `3f80::a00:0:0:0:845:4567`: no es correcta porque le agrega un campo de más.

* `3f80:0:0:a00::845`: correcta.

* `3f8:0:0:a00::845`: no es correcta porque al sacar el *"0"* después de *"3f8"* estás modificando el hexadecimal.

# 10. Indique si las siguientes direcciones son de link-local, global-address, multicast, etc.

Para identificar direcciones:
* **Link-Local:** su prefijo es `fe80::/64` o `fe80::/10`. En bits, serían los primeros 16 en `1111 1110 1000 0000`. 
* **Multicast:** su prefijo es `ff00::/8`. En bits, serían los primeros 8 en `1111 1111`.
* **Globales:** su prefijo es `2000::/3`. En bits, serían los primeros 3 en `001`.

##

* `fe80::1/64`: Link-local.
* `3ffe:4543:2:100:4398::1/64`: Global-address.
* `::`: Anycast.
* `::1`: Anycast.
* `ff02::2`: Multicast.
* `2818:edbc:43e1::8721:122`: Global-address.
* `ff02::9`: Multicast.

# 11. Dado el siguiente diagrama, ¿qué direcciones IPv6 será capaz de autoconfigurar el nodo A en cada una de sus interfaces?

<img src="./screenshots/Practica 9/ej11.png">

## `eth0`
* 0000 0000 0000 0000:1b:77:b1:49:a1
* 0000 0010 0000 0000:1b:77:b1:49:a1
* 200:1b:77:b1:49:a1
* 200:1b:77:ff:fe:b1:49:a1
* fe80::200:1b:77:ff:fe:b1:49:a1

fe80::21b:77ff:feb1:49a1

## `eth1`

* 0000 0000 1100 0000:25:ee:ba:93:e1
* 0000 0010 1100 0000:25:ee:ba:93:e1
* 2c0:25:ee:ba:93:e1
* 2c0:25:ee:ff:fe:ba:93:e1
* fe80::2c0:25:ee:ff:fe:ba:93:e1

fe80::2e5:eeff:feba:93e1

## 12. Al autogenerarse una dirección IPv6 sus últimos 64 bits en muchas ocasiones no se deducen de la dirección MAC, se generan de forma random, ¿por qué sucede esto? ¿Qué es lo que se intenta evitar?

La autogeneración aleatoria de las direcciones IPv6 se decidió en el RFC 8981. Además de generarse aleatoriamente, estas direcciones son temporales, por lo que caducan cada cierto tiempo y deben reemplazarse por nuevas.

El problema original surge porque las direcciones MAC son estáticas y únicas, por lo que al conectarse a la red con una dirección IPv6 deducida a partir de la MAC haría que un usuario sea muy fácil de rastrear y seguir. Gracias a la autogeneración aleatoria y las direcciones temporales, el rastreamiento y seguimiento de una dirección a través de la red se vuelve difícil, o casi imposible.