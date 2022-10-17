# 1) ¿Qué servicios presta la capa de red? ¿Cuál es la PDU en esta capa? ¿Qué dispositivo es considerado sólo de la capa de red?

La capa de red es la que permite la conectividad y enrutamiento entre dos o más hosts diferentes en una red. El PDU de esta capa es el *datagrama*.

El router es el único dispositivo de la red que sólo implementa protocolos de la capa de red hacia abajo; no tiene protocolos de la capa de aplicación ni de la capa de transporte. Cada router contiene una *tabla de ruteo*, con la cual decide hacia qué nodo enviar el mensaje para que este siga su camino hasta llegar al destino deseado.

# 2) ¿Por qué se lo considera un protocolo de mejor esfuerzo?

Se dice que IP es un protocolo de *best-effort* porque va a intentar hacer todo lo posible para que todos los paquetes lleguen al destino y en orden, pero no puede garantizar que lo hagan. Los usuarios van a recibir el mejor servicio posible, pero el ancho de banda y los tiempos de respuesta van a estar condicionados por el tráfico de la red.

# 3) ¿Cuántas redes clase A, B y C hay? ¿Cuántos hosts como máximo pueden tener cada una?

Las redes se identifican a través de su Net ID; el Net ID está formado por una cantidad de octetos dependiendo de la clase de la dirección. Los octetos restantes se usan para diferenciar a un host dentro de la red.

Se puede identificar la clase de una dirección IP según el valor de su primer octeto:
* `Clase A`: 1-126. Usa un campo para Net ID y 3 para Host ID.
* `Clase B`: 128-191. Usa 2 campos para Net ID y 2 para Host ID.
* `Clase C`: 192-223. Usa 3 campos para Net ID y 1 para Host ID.

|  | Cant. de redes | Host máximos |
| --- | --- | --- |
| Clase A | 128 | 16777214 |
| Clase B | 16382 | 65534 |
| Clase C | 2097150 | 254 |

La cantidad de redes para cada clase depende del intervalo reservado para el primer octeto y la cantidad de octetos restantes asignados para el Net ID.

La cantidad máxima de hosts para cada clase se calcula como calculándose como *2<sup> N</sup> - 2*, siendo N la cantidad de bits usados para el Host ID; la cantidad de bits la podemos obtener multiplicando la cantidad de octetos del Host ID por 2.

Al resultado de *2<sup> N</sup>* se le restan las dos direcciones reservadas para la red (último octeto en 0) y para el broadcast (último octeto en 1).

# 4) ¿Qué son las subredes? ¿Por qué es importante siempre especificar la máscara de subred asociada?

Una subred es un rango lógico de direcciones que se usa para dividir una red muy grande. Esto permite, entre otras cosas, hacer la red más manejable y controlar el tráfico.

La máscara de la subred indica qué bits de la dirección IP corresponden al Net ID y qué otros corresponden al Host ID.

# 5) ¿Cuál es la finalidad del campo Protocol en la cabecera IP? ¿A qué campos de la capa de transporte se asemeja en su funcionalidad?

El campo Protocol se usa sólo cuando un datagrama IP alcanza su destino. El valor de este campo indica el protocolo de la capa de transporte al que se pasarán los datos contenidos.

El número de protocolo de un datagrama IP es similar al número de puerto destino en un segmento de la capa de transporte. El número de protocolo sirve para enlazar las capas de red y de transporte, mientras que el número de puerto enlaza a la capa de transporte con la de aplicación.

# 6) Para cada una de las siguientes direcciones IP (172.16.58.223/26, 163.10.5.49/27, 128.10.1.0/23, 10.1.0.0/24, 8.40.11.179/12) determine:

| | Clase | Subred | Hosts | Broadcast | Rango |
| --- | --- | --- | --- | --- | --- |
| 172.16.58.223 | B | 172.16.58.192 | 62 | 172.16.58.255 |172.16.58.193 - 172.16.58.254 |
| 163.10.5.49 | B | 163.10.5.32 | 30 | 163.10.5.63 | 163.10.5.33 - 163.10.5.62 |
| 128.10.1.0 | B | 128.10.0.0 | 510 | 128.10.1.255 | 128.10.0.1 - 128.10.1.254 |
| 10.1.0.0 | A | 10.1.0.0 | 254 | 10.1.0.255 | 10.1.0.1 - 10.1.0.254 |
| 8.40.11.179 | A | 8.32.0.0 | 1048574 | 8.47.255.255 | 8.32.0.1 -  8.47.255.254 |

# 7) Su organización cuenta con la dirección de red 128.50.10.0. Indique:
## a. ¿Es una dirección de red o de host?

*// No estoy seguro de esta (CONSULTAR)*

Es una dirección de host, puesto que el Host ID está indicando a una dirección concreta. Si fuese una dirección de red sólo tendría datos en el Net ID.

## b. Clase a la que pertenece y máscara de clase.

Pertenece a la Clase **B**. La máscara de esta clase es `255.255.0.0`

## c. Cantidad de hosts posibles.

*// No sé si se refiere a la dirección esta o si a la Clase*

Las direcciones de Clase **B** tienen 65534 Hosts posibles.

## d. Se necesitan crear, al menos, 513 subredes. Indique:

### i. Máscara necesaria.

A la máscara de la Clase vamos a tener que agregarle 10 bits más para que cubra las 513 subredes.

La máscara sería `11111111.11111111.11111111.11000000`, o traducida a decimal **255.255.255.192**.

### ii. Cantidad de redes asignables.

Con los 10 bits para la subred podemos asignar 1024 redes.

### iii. Cantidad de hosts por subred.

De los 32 bits de la dirección, en cada subred van a quedar 6 bits para los hosts.

La cantidad de hosts por subred va a ser de 62.

### iv. Dirección de la subred 710.

La dirección es `128.50.177.64`.

Para encontrar la dirección de una N-ésima subred hay que obtener el número N - 1 en binario y ponerlo en los bits de subred de la máscara. Luego hacemos la conversión de la dirección en bits a decimal, separando los octetos.

*(La dirección IP en bits era `11111111.11111111.10110001.01111111`)*

### v. Dirección de broadcast de la subred 710.

La dirección es `128.50.177.127`.

Una vez que obtenemos la dirección en bits de la subred, para obtener la dirección de broadcast tenemos que poner los bits reservados para el Host en 1 (en este caso quedaban los últimos 6).

# 8) Si usted estuviese a cargo de la administración del bloque IP 195.200.45.0/24
## a. ¿Qué máscara utilizaría si necesita definir al menos 9 subredes?

Para la máscara vamos a precisar 4 bits para definir a la subred (2 <sup>4</sup> = 16).

La máscara sería `255.255.255.240/28` (*11111111.11111111.11111111.11110000*)

## b. Indique la dirección de subred de las primeras 9 subredes.

* 1: 195.200.45.0
* 2: 195.200.45.16
* 3: 195.200.45.32
* 4: 195.200.45.48
* 5: 195.200.45.64
* 6: 195.200.45.80
* 7: 195.200.45.96
* 8: 195.200.45.112
* 9: 195.200.45.128

## c. Seleccione una e indique dirección de broadcast y rango de direcciones asignables en esa subred.

Dada la subred 9 ( *195.200.45.128* ):

* Broadcast: `195.200.45.143`
* Rango: `195.200.45.129 - 195.200.45.142`

# 9) Dado el siguiente gráfico:

<img src="./screenshots/Practica 7/ej9.png">

## a. Verifique si es correcta la asignación de direcciones IP y, en caso de no serlo, modifique la misma para que lo sea.

Algunos de los errores son:
* La dirección IP `172.26.22.3` corresponde al broadcast de la subred. La dirección correcta sería la que representa a la red (`172.26.22.0`)
* La dirección `172.17.10.17` está fuera del rango de la subred 172.17.10.0.

## b. ¿Cuántos bits se tomaron para hacer subredes en la red 10.0.10.0/24? ¿Cuántas subredes se podrían generar?

Se usaron 16 bits para la subredes. Se podrían hacer 2<sup> 16</sup> subredes (65536).

## c. Para cada una de las redes utilizadas indique si son públicas o privadas.

| Públicas | Privadas |
| --- | --- |
| 191.26.145.0 | 172.26.22.0 |
| | 172.17.10.0 |
| | 10.0.10.0 |
| | 192.168.5.0 |

* IPs privadas de Clase A: `10.0.0.0 – 10.255.255.255 `
* IPs privadas de Clase B: `172.16.0.0 – 172.31.255.255`
* IPs privadas de Clase C: `192.168.0.0 – 192.168.255.25`

# 10) ¿Qué es CIDR (Class Interdomain routing)? ¿Por qué resulta útil?

CIDR es una estrategia de routing para mejorar el modo de interpretar las direcciones IP. Su uso permitió un uso más eficiente de las direcciones IPv4 y una mejora de la jerarquía de direcciones, que reduce la sobrecarga de los routers.

En vez de usar la sintáxis de clases para nombrar las direcciones IP, CIDR usa la técnica de VLSM (*Variable Length Subnet Mask*) para asignar prefijos de longitud aribtraria.

# 11) ¿Cómo publicaría un router las siguientes redes si se aplica CIDR?

## a. 198.10.1.0/24
## b. 198.10.0.0/24
## c. 198.10.3.0/24
## d. 198.10.2.0/24

# 12) Listar las redes involucradas en los siguientes bloques CIDR:

## 200.56.168.0/21
## 195.24.0.0/13
## 195.24/13

# 13) El bloque CIDR 128.0.0.0/2 o 128/2, ¿Equivale a listar todas las direcciones de red de clase B? ¿Cuál sería el bloque CIDR que agrupa todas las redes de clase A?



# 14) ¿Qué es y para qué se usa VLSM?

VLSM (*Variable-Length Subnet Mask*) es una estrategia de división de subredes. Se realizan varios niveles de división de subredes para ajustar la cantidad de Hosts para cada segmento de la red.

Antes de VLSM se usaban máscaras de tamaño fijo, con las que todas las subredes iban a tener la misma cantidad de Hosts, aunque sólo utilizasen unos pocos. Esto provocaba un desperdicio muy grande de direcciones IP.

# 15) Describa, con sus palabras, el mecanismo para dividir subredes utilizando VLSM.

* `Paso 1`: se realiza la división de subredes para la red.
* `Paso 2`: se le asigna uno de los segmentos a la red que necesita los Hosts.
* `Paso 3`: se realiza la división de subredes con el segmento restante del paso anterior.
* `Paso 4`: vuelve al paso 2 y continúa el ciclo hasta distribuir totalmente los Hosts.

# 16) Suponga que trabaja en una organización que tiene la red que se ve en el gráfico y debe armar el direccionamiento para la misma, minimizando el desperdicio de direcciones IP. Dicha organización posee la red 205.10.192.0/19, que es la que usted deberá utilizar

<img src="./screenshots/Practica 7/ej16.png">

## a. ¿Es posible asignar las subredes correspondientes a la topología utilizando subnetting sin VLSM? Indique la cantidad de hosts que se desperdicia en cada subred.



## b. Asigne direcciones a todas las redes de la topología. Tome siempre en cada paso la primer dirección de red posible.



## c. Para mantener el orden y el inventario de direcciones disponibles, haga un listado de todas las direcciones libres que le quedaron, agrupándolas utilizando CIDR.



## d. Asigne direcciones IP a todas las interfaces de la topología que sea posible.



# 17) Utilizando la siguiente topología y el bloque asignado, arme el plan de direccionamiento IPv4 teniendo en cuenta las siguientes restricciones: *Utilizar el bloque IPv4 200.100.8.0/22*.

<img src="./screenshots/Practica 7/ej17.png">

## a. La red A tiene 125 hosts y se espera un crecimiento máximo de 20 hosts.



## b. La red X tiene 63 hosts.



## c. La red B cuenta con 60 hosts



## d. La red Y tiene 46 hosts y se espera un crecimiento máximo de 18 hosts.



## e. En cada red, se debe desperciciar la menor cantidad de direcciones IP posibles. En este sentido, las redes utilizadas para conectar los routers deberán utilizar segmentos de red /30 de modo de desperdiciar la menor cantidad posible de direcciones IP

# 18) Asigne direcciones IP en los equipos de la topología según el plan anterior.

# 19) Describa qué es y para qué sirve el protocolo ICMP.

ICMP es el *Protocolo de Control de Mensajes de Internet*. Es un protocolo usado para enviar mensajes de error o información de estado sobre los Hosts y servicios de la red.

Se le considera un protocolo de la capa de red, aunque en realidad los mensajes ICMP son encapsulados en datagramas IP y se envían hacia el destino usando las direcciones de este protocolo.

## a. Analice cómo funciona el comando ping.

El comando `ping` envía un mensaje ICMP al host especificado; si está disponible, le va a devolver una respuesta ICMP. Se usa para verificar las conexiones entre un Host y otro, viendo cosas como la latencia o la pérdida de paquetes.

### i. Indique el tipo y código ICMP que usa el ping.

Ping envía un mensaje ICMP de "eco", de tipo 8 y con código 0.

### ii. Indique el tipo y código ICMP que usa la respuesta de un ping.

El host responde al Ping con un mensaje ICMP de tipo 0 y con código 0.

## b. Analice cómo funcionan comandos como traceroute/tracert de Linux/Windows y cómo manipulan el campo TTL de los paquetes IP.



## c. Indique la cantidad de saltos realizados desde su computadora hasta el sitio www.nasa.gov. Analice:

### i. Cómo hacer para que no muestre el nombre del dominio asociado a la IP de cada salta.



### ii. La razón de la aparición de * en parte o toda la respuesta de un salto.



## d. Verifique el recorrido hacia los servidores de nombre del dominio unlp.edu.ar. En base al recorrido realizado, ¿podría confirmar cuál de ellos toma un camino distinto?



# 20) ¿Para que se usa el bloque 127.0.0.0/8? ¿Qué PC responde a los siguientes comandos?
## a. ping 127.0.0.1
## b. ping 127.0.54.43

# 21) Investigue para qué sirven los comandos ifconfig y route. ¿Qué comandos podría utilizar en su reemplazo? Inicie una topología con CORE, cree una máquina y utilice en ella los comandos anteriores para practicar sus diferentes opciones, mínimamente:

### Configurar y quitar una dirección IP en una interfaz. 
### Ver la tabla de ruteo de la máquina.