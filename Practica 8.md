# 2. Se tiene la siguiente red con los MTUs indicados en la misma. Si desde pc1 se envía un paquete IP a `pc2` con un tamaño total de 1500 bytes (cabecera IP más payload) con el campo Identification = 20543, responder:
## Indicar IPs origen y destino y campos correspondientes a la fragmentación cuando el paquete sale de `pc1`

<img src="./screenshots/Practica 8/ej2.png">

Los flags relacionados a la fragmentación son:
* `Identification`: representa al datagrama que se está enviando; si se tiene que fragmentar, todos los fragmentos van a tener el mismo ID.
* `DF (Do not Fragment)`: flag que si está en 1 indica que el datagrama no se debe fragmentar.
* `MF (More Fragments)`: flag que si está en 1 indica que vienen más fragmentos que forman parte del mismo mensaje.
* `Fragment Offset`: indica a partir de qué byte deben insertarse los datos enviados en este fragmento. Si el datagrama no está fragmentado, su valor es siempre 0.

| Campo | Valor |
| --- | --- |
| IP Origen | 10.0.0.20/24 |
| IP Destino |  10.0.2.20/24 |
| Identification | 20543 |
| Length | 1500 |
| DF | 0 |
| MF | 0 |
| Offset | 0 |

## ¿Qué sucede cuando el paquete debe ser reenviado por el router R1?

El enlace entre R1 y R2 tiene un MTU de 600B, que es menor al tamaño del paquete que estamos enviando. Por lo tanto, el paquete va a tener que fragmentarse y ser enviado en varios datagramas.

## Indicar cómo quedarían las paquetes fragmentados para ser enviados por el enlace entre R1 y R2.

El datagrama original tiene un tamaño de datos de 1480B (1500B totales - 20B del header). Teniendo en cuenta que el enlace tiene un MTU de 600B, podemos dividir el datagrama en fragmentos de 532B (512B de datos + 20B del header), aunque el último quedaría de 476B.

#### Fragmento 1

| Campo | Valor |
| --- | --- |
| IP Origen | 10.0.0.20/24 |
| IP Destino |  10.0.2.20/24 |
| Identification | 20543 |
| Length | 532 |
| DF | 0 |
| MF | 1 |
| Offset | 0 |

#### Fragmento 2

| Campo | Valor |
| --- | --- |
| IP Origen | 10.0.0.20/24 |
| IP Destino |  10.0.2.20/24 |
| Identification | 20543 |
| Length | 532 |
| DF | 0 |
| MF | 1 |
| Offset | 64 |

#### Fragmento 3

| Campo | Valor |
| --- | --- |
| IP Origen | 10.0.0.20/24 |
| IP Destino |  10.0.2.20/24 |
| Identification | 20543 |
| Length | 476 |
| DF | 0 |
| MF | 0 |
| Offset | 128 |


*// ALGUNAS ANOTACIONES DE COSAS PARA ACORDARME*

* El offset de los fragmentos intermedios (y del final) se calcula como la suma del tamaño de datos de los fragmentos anteriores dividido por 8.
* El primer fragmento siempre tiene el offset en 0.
* Los campos de IP Origen, IP Destino y Identification no cambian en ningún fragmento.
* El fragmento final se caracteriza porque es el único que tiene el flag de MF en 0.

## ¿Dónde se unen nuevamente los fragmentos? ¿Qué sucede si un fragmento no llega?

Los fragmentos son reensamblados en los sistemas terminales; en este caso, en PC2.

Si se pierde un fragmento, se deben retransmitir todos los fragmentos del paquete original. Sin embargo, IP no tiene mecanismos para comprobar la llegada de los fragmentos, así que depende de las decisiones de los protocolos de las capas superiores.

## Si un fragmento tiene que ser reenviado por un enlace con un MTU menor al tamaño del fragmento, ¿qué hará el router con ese fragmento?

Un fragmento puede volver a ser fragmentado si el enlace por el que se envía tiene un MTU menor a su tamaño total.

# 3. ¿Qué es el ruteo? ¿Por qué es necesario?

El ruteo consiste en el camino que hace un datagrama llega desde un origen hasta un destino a través de la red. Este recorrido incluye todos los nodos intermedios de red por los que debe pasar y los cuales retransmiten la información a otros.

El ruteo es lo que permite que un paquete se transmita a través de la red, y sepa qué ruta seguir para llegar al destino deseado.

# 4. En las redes IP el ruteo puede configurarse en forma estática o en forma dinámica. Indique ventajas y desventajas de cada método.

| Ruteo estático | Ruteo dinámico |
| -------------- | -------------- |
| Las tablas de enrutamiento de cada nodo se configuran manualmente. |  Requiere una configuración inicial por el administrador de la red. |
| Las tablas no pueden adaptarse en tiempo real a los cambios de topología la red. | Se adapta automáticamente a los cambios de topología. |
| El cálculo de la ruta óptima es offline, por lo que no importa su complejidad ni su tiempo. | Encuentra las rutas óptimas teniendo en cuenta información obtenida por el protocolo. |
| No es escalable ni tolerante a fallos. Es útil para redes sencillas. |  Es más escalable y tolerante a fallos, pero la resolución de problemas y el debugging es más complejo. |

# 5. Una máquina conectada a una red pero no a Internet, ¿tiene tabla de ruteo?

*// NO ESTOY SEGURO PERO*

Los sistemas operativos tienen una tabla de ruteo propia, por lo que aunque no estén conectados a la red pueden verla, aunque no la van a poder utilizar.

# 6. Observando el siguiente gráfico y la tabla de ruteo del router D, responder:

<img src="./screenshots/Practica 8/ej6.png">

## a. ¿Está correcta esa tabla de ruteo? En caso de no estarlo, indicar el o los errores encontrados. Escribir la tabla correctamente (no es necesario agregar las redes que conectan contra los ISPs).

| Entrada (Destino > Next-Hop) | Error | Solución |
| ------- | ----- | ----- |
| 10.0.0.12 > 10.0.0.5/30 | Las direcciones del Next-Hop no llevan máscara.| Quitar el `/30` de la dirección del Next-Hop  y dejar sólo el `10.0.0.5`.
| 205.10.128.0 > 10.0.0.2 | La dirección `10.0.0.2` corresponde al Router D dentro de la red de 10.0.0.0/30 (lo vemos porque al final del tramo hay un `.2`). | Modificar el Next-Hop a la dirección del Router A, `10.0.0.1`. |
| 205.20.0.193 > 10.0.0.1 | La dirección destino `205.20.0.193` no es una dirección de red, si no que es de Host. |  Reemplazar la red destino por la dirección de red; en este caso, `205.20.0.192/26` (Red B). |
| N/A | Falta la entrada con Red Destino `10.0.0.8/30`. | Agregar una entrada con Red Destino `10.0.0.8`, Mask `/30`, Next-Hop `-`. |

## b. Con la tabla de ruteo del punto anterior, Red D, ¿tiene salida a Internet? ¿Por qué? ¿Cómo lo solucionaría? Suponga que los demás routers están correctamente configurados, con salida a Internet y que Rtr-D debe salir a Internet por Rtr-C.

*// NO ESTOY SEGURO PERO*

La tabla de ruteo de Red D no tiene ninguna entrada con destino a los ISP, por lo que no puede acceder a internet; para solucionarlo habría que agregar dicha entrada.

Considerando la salida a Internet por `Rtr-C`, habría que agregar la siguiente entrada a la tabla:

| Red destino | Mask | Next-Hop | Iface |
| ----------- | ---- | -------- | ----- |
| 130.0.10.0  | /30  | 10.0.0.10 | Acá no sé qué va xd |

## c. Teniendo en cuenta lo aplicado en el punto anterior, si en Rtr-C estuviese la siguiente entrada en su tabla de ruteo qué sucedería si desde una PC en Red D se quiere acceder un servidor con IP `163.10.5.15`.

```
|  Red Destino  |  Mask  |  Next-Hop  | Iface |
|  -----------  |  ----  |  --------  | ----- |
|  163.10.5.0   |  /24   |  10.0.0.9  | eth1  |
```

## d. ¿Es posible aplicar sumarización en esa tabla, la del router Rtr-D? ¿Por qué? ¿Qué debería suceder para poder aplicarla?

La sumarización (en el libro está como *agregación*) consiste en unificar varias redes individuales en una sola entrada de la tabla de ruteo; es algo similar al CIDR del subnetting, pero aplicado a la tabla de ruteo de cada router.

En Rtr-D hay dos casos posibles de sumarización: las redes `10.0.0.4 - 10.0.0.0` y las redes `205.20.0.128 - 205.20.0.192`; sin embargo, ninguno de los dos casos pueden sumarizarse.

* En el primer caso no se puede porque tienen distinta interfaz.
* En el segundo caso no se puede porque cada uno hace un salto distinto.

No se puede sumarizar si las entradas tienen distinta información o si se pierde información al sumarizar. Para que se pueda aplicar sobre esta tabla, las redes `205.20.0.128 - 205.20.0.192` tendrían que seguir el mismo camino.

## e. La sumarización aplicada en el punto anterior, ¿se podría aplicar en Rtr-B? ¿Por qué?

*// LO TENGO QUE REVISAR XD*

## f. Escriba la tabla de ruteo de Rtr-B teniendo en cuenta lo siguiente:
* Debe llegarse a todas las redes del gráfico
*  Debe salir a Internet por Rtr-A
* Debe pasar por Rtr-D para llegar a Red D
* Sumarizar si es posible

| Red Destino | Mask | Next-Hop | Iface |
| ----------- | ---- | -------- | ----- |
| 205.20.0.128 | /25 |  - | Como está sumarizado no sé cual de los 2 eth iría |
| 10.0.0.12 | /30 | - | eth3 |
| 10.0.0.4 | /30 | - | eth1 |
| 10.0.0.0 | /30 | 10.0.0.13 | eth3 |
| 10.0.0.16 | /30 | 10.0.0.13 | eth3 |
| 10.0.0.8 | /30 | 10.0.0.6 | eth1 |
| 153.10.20.128 | /27 | 10.0.0.6 | eth2 |
| 205.10.0.128 | /25 | 10.0.0.13 | eth0 |
| 153.10.20.128 | /27 | 10.0.0.6 | eth1 |
| 163.10.5.75 | /27 | 10.0.0.6 | eth1 |
| 205.10.0.128 | /25 | 10.0.0.13 | eth3 |
| 120.0.0.0 | /30 | 10.0.0.13 | eth3 |

## g. Si Rtr-C pierde conectividad contra ISP-2, ¿es posible restablecer el acceso a Internet sin esperar a que vuelva la conectividad entre esos dispositivos?

Podría hacerse un camino más largo, en el que todos los dispositivos lleguen a la red a través de ISP-1 y pasando por `Rtr-A`. Sin embargo, es u ncamino más largo para algunos routers y requiere actualizar las tablas de ruteo en el caso de que no conozcan la Red Destino `120.0.0.0/30`.

# 7. Evalúe para cada caso si el mensaje llegará a destino, saltos que tomará y tipo de respuesta recibida el emisor

<img src="./screenshots/Practica 8/ej7.png">

## a. Un mensaje ICMP enviado por PC-B a PC-C.
## b. Un mensaje ICMP enviado por PC-C a PC-B.
## c. Un mensaje ICMP enviado por PC-C a 8.8.8.8.
## d. Un mensaje ICMP enviado por PC-B a 8.8.8.8.
