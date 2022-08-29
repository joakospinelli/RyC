# 1) ¿Qué es una red? ¿Cuál es el principal objetivo para construir una red?
Una red es un conjunto de dispositivos o componentes conectados entre sí. Su objetivo es el de compartir recursos, comunicar información y ofrecer servicios.

# 2) ¿Qué es Internet? Describa los principales componentes que permiten su funcionamiento
Internet es un tipo de red descentralizada que usa el protocolo TCP/IP. Está formada por un conjunto de diversas redes, que gracias a usar el mismo protocolo común son unificadas en una gran red de alcance casi mundial, que lógicamente actúa como una.

Uno de los principales servicios de Internet y el que produció su éxito (World Wide Web), que permite la transferencia de archivos para acceder a las páginas web.

Está formada por varios protocolos de comunicación; entre los más conocidos están HTTP (para la transferencia mediante WWW), SMTP (para correos electrónicos), FTP y P2P (para transferencia de archivos), entre otros.

Los componentes que forman Internet son:
* **Clientes:** dispositivos de los que se conectan los usuarios finales. Incluyen las aplicaciones mediante las que se realizan las peticiones a los servidores (por ejemplo, navegadores Web)
* **Servidores:** equipos cargados con programas que ofrecen servicios a los clientes. Estos captan las solicitudes que se les realizan y brindan una respuesta adecuada. Puede haber varios clientes consultando a un único servidor, pero la respuesta siempre se realiza a un único cliente.
* **Proveedores de internet:** generalmente son empresas que le brindan el servicio de Internet a los usuarios finales. Estas empresas se encargan de establecer los nodos que conectan a todas las computadoras entre sí.

# 3) ¿Qué son las RFC?
RFC son una serie de publicaciones por ingenieros de Internet para describir el funcionamiento de Internet y otro tipo de redes de computadoras. Incluye una serie de normas o estándares para hacer protocolos de red que puedan ser usados e interpretados por distintos tipos de sistemas, independientemente de su Sistema Operativo.

Varios protocolos de los más usados actualmente están definidos según los estándares de RFC (HTTP, FTP, IP, etc.)

# 4) ¿Qué es un protocolo?
Un protocolo de red es un conjunto de normas que establecen cómo debe realizarse la conexión entre los dispositivos conectados a esa red.

Define elementos como el formato y orden de los mensajes, o las acciones a realizar cuando hay una emisión/recepción entre dispositivos. Están definidos directamente sobre los componentes.

# 5) ¿Por qué dos máquinas con distintos sistemas operativos pueden formar parte de una misma red?
Los protocolos de red pueden definirse en cualquier tipo de máquina, independientemente de su Sistema Operativo. Esto se implementó para resolver los problemas de compatibilidad que traían los primeros sistemas con redes, puesto que sólo podían conectarse con otros sistemas del mismo tipo.

Gracias a esta estandarización, mientras dos máquinas reconozcan los protocolos definidos por una red, pueden comunicarse a través de ella independientemente del SO que usen.

# 6) ¿Cuáles son las 2 categorías en las que pueden clasificarse a los sistemas finales o End Systems? Dé unejemplo del rol de cada uno en alguna aplicación distribuida que corra sobre Internet.
Los sistemas finales de una red pueden ser:
* **Clientes:** sistemas que solicitan servicios a los servidores y esperan una respuesta de ellos.
* **Servidores:** sistemas que esperan solicitudes de servicios por parte de los clientes para recibirlas, procesarlas y responderlas.

Un ejemplo de esto en una arquitectura distribuida serían las páginas Web. El cliente estaría representado por el navegador, que solicita la página para mostrarla al usuario. El servidor está representado por el servidor Web en sí, que es el encargado de recibir las peticiones del navegador y retornar los valores que se soliciten.

# 7) ¿Cuál es la diferencia entre una red conmutada de paquetes de una red conmutada de circuitos?
* En las **redes conmutadas por paquetes** el cliente va a recibir la información del servidor mediante paquetes del mismo tamaño. Cada paquete tiene cabeceras con información como el origen, el destinatario y datos de control para su procesamiento. Los paquetes se envían por un canal compartido por varios clientes a la vez, y el cliente puede recibirlos desordenados, por lo que debe “ensamblar” el mensaje una vez que reciba todos.
* En las **redes conmutadas por circuitos** los equipos establecen la conexión mediante un canal físico único, que es bloqueado hasta que termina toda la conexión.

La conmutación por paquetes es más efectiva para transferencias de datos que pueden permitir atrasarse y no ser en tiempo real (Por ejemplo, envío de emails o páginas Web). La conmutación por circuitos es más efectiva para transferencias directas que requieran de actualización y comunicaciones en tiempo real (Por ejemplo, llamadas o transmisiones)

# 8) Analice qué tipo de red es una red de telefonía y qué tipo de red es Internet.
Una red de telefonía es conmutada por circuitos, puesto que se establece un canal único entre los dispositivos que no se libera hasta que la llamada termina.
Internet es una red conmutada por paquetes porque permite la conexión de varios clientes a un único servidor. El cliente que recibe la conexión es el encargado de ordenar los paquetes para mostrar la información correspondiente.

# 9) Describa brevemente las distintas alternativas que conoce para acceder a Internet en su hogar.
El acceso a Internet se le solicita a un proveedor de Internet (ISP) que nos brinda el servicio de conexión, que puede llegar a nuestra casa a través de diversos medios (por ejemplo, cable coaxial o fibra óptica). Dentro de la casa, los dispositivos pueden conectarse mediante Wi-Fi a través de módem, o a través de Ethernet mediante un cable de red. Los dispositivos pueden acceder a Internet independientemente de su tipo o sistema operativo; pueden ser computadoras, teléfonos, televisores, dispositivos con IoT, etc.

# 10) ¿Qué ventajas tiene una implementación basada en capas o niveles?
Una implementación por capas o niveles tiene las siguientes ventajas:
* Protege a las capas inferiores de su acceso directo, gracias a la intervención de las capas superiores.
* Abstrae al programador de la complejidad de las capas inferiores, permitiendo enfocarse en las superiores.
* Permite el intercambio o complementación de la información y operaciones al pasar por cada una de las capas.
* Es más escalable, fácil de programar y mantener gracias a la separación de componentes.

# 11) ¿Cómo se llama la PDU de cada una de las siguientes capas: Aplicación, Transporte, Red y Enlace?
La PDU (Process Data Unit) es la unidad mínima del mensaje que se transmite en cada una de las capas. El mensaje inicial de la capa superior va cambiando a medida que pasa por las inferiores; se modifica y se le agrega información. El nombre del mensaje y su formato también cambia en cada capa:

| Capa | Nombre del mensaje |
| ------------- | ------------- |
| Capa de Aplicación | Mensaje |
| Capa de Transporte | Segmento |
| Capa de Red | Datagrama |
| Capa de Enlace | Trama |

# 12) ¿Qué es la encapsulación? Si una capa realiza la encapsulación de datos, ¿qué capa del nodo receptor realizará el proceso inverso?
La encapsulación de datos consiste en el agregado y encriptado de información que cada capa realiza al mensaje original, antes de pasar a la siguiente capa.

Cuando un nodo emisor termina de procesar el mensaje y lo envía al nodo receptor, cada capa del receptor se encarga de desencapsular el mensaje que encapsuló esa misma capa en el emisor.

# 13) Describa cuáles son las funciones de cada una de las capas del stack TCP/IP o protocolo de Internet.

| Capa | Descripción |
| --- | --- |
| Aplicación | Presenta la interfaz para los sistemas finales en la que se solicita la conexión para transferir la información.
| Transporte | Se establecen los canales necesarios para la transferencia de la información. Desde el emisor, envía la dirección de destino a la capa de red del host receptor. |
| Red | Mueve los paquetes a través de la red, de un host a otro. |
| Enlace | Distribuye los datagramas de la capa de red a través de los nodos Internet intermedios hasta llegar al host destino. |
| Física | Mueve físicamente los bits de un nodo a otro. Dependen del medio físico en el que se transmite la red (coaxial, fibra óptica, etc.) |

# 14) Compare el modelo OSI con la implementación TCP/IP.
El modelo OSI fue el primer modelo para protocolos de red. Tiene 7 capas:
* Aplicación
* Presentación
* Sesión
* Transporte
* Red
* Enlace
* Física

El modelo TCP/IP lo que hace es agrupar las capas de Aplicación, Presentación y Sesión en una sola (la capa de aplicación), por lo que termina teniendo únicamente 5 capas.
