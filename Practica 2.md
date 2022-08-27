# 2) ¿Cuál es la función de la capa de aplicación?
En la capa de aplicación se encuentran las interfaces para que los sistemas finales establezcan las comunicaciones entre ellos. Es la capa que permite a los procesos acceder a los servicios de todas las anteriores.

# 3) Si dos procesos deben comunicarse:
## a. ¿Cómo podrían hacerlo si están en diferentes máquinas?
Pueden comunicarse intercambiando mensajes a través de una red de computadoras. En esta red, un proceso pasa a ser el “emisor” del mensaje, que lo envía a la red para que el otro proceso “receptor” lo reciba.

## b. Y si están en la misma máquina, ¿qué alternativas existen?
Dentro de la misma computadora, pueden comunicarse compartiendo información en regiones de memoria compartida o comunicándose mediante mensajes en canales designados por la arquitectura.

# 4) Explique brevemente cómo es el modelo Cliente/Servidor. De un ejemplo de un sistema Cliente/Servidor en la “vida cotidiana” y un ejemplo de un sistema informático que siga el modelo Cliente/Servidor. ¿Conoce algún otro modelo de comunicación?

El modelo cliente/servidor se usa para realizar la comunicación entre dos procesos. Como su nombre indica, hay dos roles:
* **Clientes:** procesos que solicitan servicios a los servidores y esperan una respuesta. Generalmente envía las solicitudes según los requerimientos del usuario final.
* **Servidores:** procesos que reciben peticiones de los clientes, las procesan y retornan una respuesta. Generalmente contiene un programa en ejecución o algún otro servicio que se encarga de gestionar las peticiones para responder acorde al estado. Puede haber varios clientes haciendo solicitudes a un único servidor, pero las respuestas son siempre hacia un único cliente.

En la “vida cotidiana” un ejemplo común de cliente/servidor sería en cualquier tienda o comercio: hay un cliente que solicita un producto, y un empleado que se encarga de entregárselo (representaría al servidor).

El ejemplo más común de sistemas informáticos con este modelo es la WWW (World Wide Web), en la que los clientes solicitan el acceso a archivos o páginas mediante los navegadores Web, y los servidores Web procesan las peticiones para retornar una respuesta.

Otros modelos de comunicación podrían ser Peer-to-peer, centralizados o simétricos.

# 5) Describa la funcionalidad de la entidad genérica “Agente de usuario” o “User agent”.
Un User Agent es cualquier aplicación que use el usuario final para acceder a una página o recurso en la World Wide Web. Esto incluye navegadores Web, teléfonos, motores de búsqueda, etc.
En el protocolo HTTP, el header User-Agent se usa para identificar datos sobre el sistema que está realizando la petición, tales como sistema operativo, compañía o versión.

# 6) ¿Qué son y en qué se diferencian HTML y HTTP?
HTML es el lenguaje de marcado de hipertexto; es un lenguaje usado para crear y dar formato a las páginas Web. HTTP es el protocolo de transferencia de hipertexto; es un protocolo de comunicación que se encuentra en la capa de aplicación, y se usa para transferir a través de la red varios tipos de archivos.
HTML es el estándar que usa la WWW para darle formato a sus páginas, mientras que HTTP es el protocolo que usa para transmitirlas y mostrarlas a los clientes.

# 7) Utilizando la VM, abra una terminal e investigue sobre el comando curl. Analice para qué sirven los siguientes parámetros (-I, -H, -X, -s).

"curl" es un comando que nos permite ver la transferencia de datos en la red usando diversos protocolos. Permite conectarse directamente con una página o servidor Web (mediante su URL o IP) y ver la respuesta obtenida; opcionalmente se le puede agregar información a la petición.

* `-I` : retorna sólamente los encabezados de las respuestas.
* `-H "Header-Name : Header-Content"` : establece un Header adicional a la petición.
* `-X "Method"` : especifica un método HTTP particular para realizar la petición. Si no se declara, el método por defecto es GET.
* `-s` : activa el modo silencioso de CURL. No va a mostrar errores o mensajes adicionales que provengan del comando, pero sí va a mostrar la respuesta a la petición.

# 8) Ejecute el comando curl sin ningún parámetro adicional y acceda a www.redes.unlp.edu.ar. Luego responda:
## a. ¿Cuántos requerimientos realizó y qué recibió? Pruebe redirigiendo la salida(>) del comando curl a un archivo con extensión html y abrirlo con un navegador.
Se realizó un único request y se obtuvo el archivo HTML que corresponde a la página.

Al abrir el archivo con el navegador, podemos ver el formato de la página Web.

## b. ¿Cómo funcionan los atributos href de los tags link e img en html?

La etiqueta `<link>` establece un vínculo entre el archivo y un recurso externo. A través de su atributo `rel` se puede especificar cómo es el vínculo entre ambos. El atributo `href` especifica la dirección del recurso que se necesita.

La etiqueta `<img>` sirve para mostrar una imagen en un documento HTML. El atributo `src` especifica dónde se encuentra la imagen, que puede estar en el mismo servidor que el documento HTML o puede ser externa.

Cuando un navegador carga una página HTML y encuentra un atributo `href`, automáticamente realiza una nueva solicitud al servidor para obtener el archivo referido.

## c. Para visualizar la página completa con imágenes como en un navegador, ¿alcanza con realizar un único requerimiento? ¿Cuántos requerimientos serían necesarios para obtener una página que tiene dos CSS, dos Javascript y tres imágenes? Diferencie como funcionaría un navegador respecto al comando curl ejecutado previamente.
Para visualizar cada imagen se requiere un requerimiento adicional.

Para cargar la página necesitaríamos 9 requerimientos:
* 2 request para los archivos de CSS,
* 2 request para los archivos de JS,
* 3 request para las imágenes,
* 1 request para el archivo HTML,
* 1 request para el favicon.

Como el comando `curl` no tiene que visualizar la página, sólamente hace una petición para obtener el archivo especificado.

Por otro lado, cuando el navegador reconoce que el archivo obtenido es HTML, necesita obtener los recursos adicionales para visualizar la página completa de la manera deseada, por lo que tiene que realizar todas las peticiones necesarias para poder cargarla totalmente. La mayoría de navegadores realizan por defecto una petición adicional para obtener el favicon, aunque no esté especificado en el HTML.

# 9) Ejecute a continuación los siguientes comandos: <br> `curl -v -s www.redes.unlp.edu.ar > /dev/null` <br> `curl -I -v -s www.redes.unlp.edu.ar`

## ¿Qué diferencias nota entre cada uno?
Ambos comandos ejecutan `curl` en modo silencioso, pero el primero redirige la respuesta a "/dev/null" y el segundo usa el parámetro `-I` para sólo ver los encabezados de la respuesta.

Como ambos están programados para no mostrar la respuesta (aunque de distinta manera), los resultados son muy similares.

## ¿Qué ocurre si en el primer comando quita la redirección a /dev/null? ¿Por qué no es necesaria en el segundo comando?
Si le quitamos la redirección vemos la respuesta a la petición en la consola de comandos, que en este caso es el archivo HTML.

En el segundo comando no hace falta porque el parámetro `-I` hace que sólo veamos los encabezados de la respuesta, pero nunca el contenido.

## ¿Cuántas cabeceras viajaron en el requerimiento? ¿Y en la respuesta?

Usando el parámetro `-I` podemos ver que los Headers fueron:

En la petición:
| Header | Contenido |
| --- | --- |
| Host | www.redes.unlp.edu.ar
| User-Agent | curl/7.74.0
| Accept | `*/*`

En la respuesta:
| Header | Contenido |
| --- | --- |
| Date | Sat, 27 Aug 2022 21:09:12 GMT |
| Server | Apache/2.4.53 (Unix) |
| Last-Modified | Wed, 13 Apr 2022 22:55:32 GMT |
| ETag | "1322-5dc9113140100" |
| Accept-Ranges | bytes |
| Content-Length | 4898 |
| Content-Type | text/html |

# 10) ¿Qué indica la cabecera Date?

"Date" indica la fecha y hora en la que se realizó la petición.

# 11) En HTTP/1.0, ¿cómo sabe el cliente que ya recibió todo el objeto solicitado completamente? ¿Y en HTTP/1.1?
??????

# 12)  Investigue los distintos tipos de códigos de retorno de un servidor web y su significado. Considere que los mismos se clasifican en categorías (2XX, 3XX, 4XX, 5XX).
Los códigos de retorno indican el estado de la respuesta y si se pudo realizar la petición correctamente. Todos los códigos tienen un número entre 100 y 500, y se dividen de la siguiente manera:
* `100-199:` Respuestas informativas, generalmente son provisionales e informan el estado de una petición pendiente.
* `200-299:` Respuestas satisfactorias. Indican el éxito de una petición y los resultados que provocó en el servidor. La más común es **200 OK**, que indica el éxito de la operación sin más detalles.
* `300-399:` Redirecciones. La más conocida es **301 Moved Permanently**, que indica que el recurso fue cambiado de lugar permanentemente; generalmente retorna la nueva URI.
* `400-499:` Errores del cliente. Indican que hubo un fallo en la comunicación debido a un problema en la petición. Los más comunes son **401 Unauthorized** (no se puede acceder a un recurso porque requiere autenticación), **404 Not Found** (no se encuentra el recurso) o **405 Method Not Allowed** (el servidor reconoce la petición pero no puede realizarla en el método solicitado).
* `500-599:` Errores del servidor. Indican que el servidor pudo recibir la petición, pero tuvo un error al procesarla que le impide dar una respuesta apropiada. Los más comunes son **500 Internal server Error** (el servidor tuvo un error al intentar responder), **501 Not Implemented** (el servidor no tiene implementada la forma de responder a esa petición) o **502 Bad Gateway** (el servidor obtuvo una respuesta inválida de otro servicio).


# 13) Utilizando curl, realice un requerimiento con el método HEAD al sitio www.redes.unlp.edu.ar e indique:
## a. ¿Qué información brinda la primer línea de la respuesta?









