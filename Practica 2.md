# 2) ¿Cuál es la función de la capa de aplicación?
En la capa de aplicación se encuentran las interfaces para que los sistemas finales establezcan las comunicaciones entre ellos. Es la capa que permite a los procesos acceder a los servicios de todas las anteriores.

# 3) Si dos procesos deben comunicarse:
## a. ¿Cómo podrían hacerlo si están en diferentes máquinas?
Pueden comunicarse intercambiando mensajes a través de una red de computadoras. En esta red, un proceso pasa a ser el “emisor” del mensaje, que lo envía a la red para que el otro proceso “receptor” lo reciba.

## b. Y si están en la misma máquina, ¿qué alternativas existen?
Dentro de la misma computadora, pueden comunicarse compartiendo información en regiones de memoria compartida o comunicándose mediante mensajes en canales designados por la arquitectura.

# 4) Explique brevemente cómo es el modelo Cliente/Servidor. De un ejemplo de un sistema Cliente/Servidor en la “vida cotidiana” y un ejemplo de un sistema informático que siga el modelo Cliente/Servidor. ¿Conoce algún otro modelo de comunicación?

El modelo cliente/servidor se usa para realizar la comunicación entre dos procesos. Como su nombre indica, hay dos roles:
* <strong>Clientes:</strong> procesos que solicitan servicios a los servidores y esperan una respuesta. Generalmente envía las solicitudes según los requerimientos del usuario final.
* <strong>Servidores:</strong> procesos que reciben peticiones de los clientes, las procesan y retornan una respuesta. Generalmente contiene un programa en ejecución o algún otro servicio que se encarga de gestionar las peticiones para responder acorde al estado. Puede haber varios clientes haciendo solicitudes a un único servidor, pero las respuestas son siempre hacia un único cliente.

En la “vida cotidiana” un ejemplo común de cliente/servidor sería en cualquier tienda o comercio: hay un cliente que solicita un producto, y un empleado que se encarga de entregárselo (representaría al servidor).

El ejemplo más común de sistemas informáticos con este modelo es la WWW (World Wide Web), en la que los clientes solicitan el acceso a archivos o páginas mediante los navegadores Web, y los servidores Web procesan las peticiones para retornar una respuesta.

Otros modelos de comunicación podrían ser Peer-to-peer, centralizados o simétricos.

# 5) Describa la funcionalidad de la entidad genérica “Agente de usuario” o “User agent”.
Un User Agent es cualquier aplicación que use el usuario final para acceder a una página o recurso en la World Wide Web. Esto incluye navegadores Web, teléfonos, motores de búsqueda, etc.
En el protocolo HTTP, el header User-Agent se usa para identificar datos sobre el sistema que está realizando la petición, tales como sistema operativo, compañía o versión.

# 6) ¿Qué son y en qué se diferencian HTML y HTTP?
HTML es el lenguaje de marcado de hipertexto; es un lenguaje usado para crear y dar formato a las páginas Web. HTTP es el protocolo de transferencia de hipertexto; es un protocolo de comunicación que se encuentra en la capa de aplicación, y se usa para transferir a través de la red varios tipos de archivos.
HTML es el estándar que usa la WWW para darle formato a sus páginas, mientras que HTTP es el protocolo que usa para transmitirlas y mostrarlas a los clientes.

# 7) 
