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
En HTTP/1.0 la conexión permanecerá hasta obtener el objeto completo.

En HTTP/1.1, si bien no se puede saber si el objeto se recibió completamente, hay algunos encabezados que pueden indicarlo:
* **Content-Length**: indica el tamaño total del objeto. Podemos usarlo para comparar con el tamaño del objeto actual.
* **Connection**: indica si la conexión se cerró totalmente o si es persistente. Si la conexión está cerrada es porque el objeto se recibió completamente.

# 12)  Investigue los distintos tipos de códigos de retorno de un servidor web y su significado. Considere que los mismos se clasifican en categorías (2XX, 3XX, 4XX, 5XX).
Los códigos de retorno indican el estado de la respuesta y si se pudo realizar la petición correctamente. Todos los códigos tienen un número entre 100 y 500, y se dividen de la siguiente manera:
* `100-199:` Respuestas informativas, generalmente son provisionales e informan el estado de una petición pendiente.
* `200-299:` Respuestas satisfactorias. Indican el éxito de una petición y los resultados que provocó en el servidor. La más común es **200 OK**, que indica el éxito de la operación sin más detalles.
* `300-399:` Redirecciones. La más conocida es **301 Moved Permanently**, que indica que el recurso fue cambiado de lugar permanentemente; generalmente retorna la nueva URI.
* `400-499:` Errores del cliente. Indican que hubo un fallo en la comunicación debido a un problema en la petición. Los más comunes son **401 Unauthorized** (no se puede acceder a un recurso porque requiere autenticación), **404 Not Found** (no se encuentra el recurso) o **405 Method Not Allowed** (el servidor reconoce la petición pero no puede realizarla en el método solicitado).
* `500-599:` Errores del servidor. Indican que el servidor pudo recibir la petición, pero tuvo un error al procesarla que le impide dar una respuesta apropiada. Los más comunes son **500 Internal server Error** (el servidor tuvo un error al intentar responder), **501 Not Implemented** (el servidor no tiene implementada la forma de responder a esa petición) o **502 Bad Gateway** (el servidor obtuvo una respuesta inválida de otro servicio).


# 13) Utilizando curl, realice un requerimiento con el método HEAD al sitio www.redes.unlp.edu.ar e indique:
<img src=".\screenshots\Practica 2\ej13.jpg"/>

## a. ¿Qué información brinda la primer línea de la respuesta?
Indica la versión de HTTP de la respuesta (HTTP 1.1) y el código de estado (200 OK).

## b. ¿Cuántos encabezados muestra la respuesta?
Muestra 7 encabezados (Date, Server, Last-Modified, ETag, Accept-Ranges, Content-Length y Content-Type).

## c. ¿Qué servidor web está sirviendo la página?
Un servidor Apache versión 2.4.53

## d. ¿El acceso a la página solicitada fue exitoso o no?
Podemos suponer que fue exitoso por el código de estado 200.

## e. ¿Cuándo fue la última vez que se modificó la página?
El 13/04/2022 a las 22:55 GMT.

## f. Solicite la página nuevamente con curl usando GET, pero esta vez indique que quiere obtenerla sólo si la misma fue modificada en una fecha posterior a la que efectivamente fue modificada. ¿Cómo lo hace? ¿Qué resultado obtuvo? ¿Puede explicar para qué sirve?

<img src="https://github.com/joakospinelli/RyC/blob/main/screenshots/Practica%202/ej13f.jpg"/>

Agregando el header `If-Modified-Since:(fecha)` al comando CURL podemos obtener una respuesta dependiendo si el archivo fue modificado antes o después de la fecha del header. Es menor-estricto.

* Si le pasamos una fecha anterior a la última fecha de modificación, obtenemos la misma respuesta.
* Si le pasamos una fecha igual o posterior, obtenemos una respuesta con el estado **304 Not Modified**.

Usando este Header podemos saber si un servidor acepta validación según la fecha de modificación.

# 14) Utilizando curl, acceda al sitio www.redes.unlp.edu.ar/restringido/index.php y siga las instrucciones y las pistas que vaya recibiendo hasta obtener la respuesta final. Será de utilidad para resolver este ejercicio poder analizar tanto el contenido de cada página como los encabezados.

Primer acceso

<img src="screenshots\Practica 2\ej14-1.jpg"/>

Segundo acceso

<img src="screenshots\Practica 2\ej14-2.jpg"/>

Tercer acceso (Agrego header `Usuario-Redes` al CURL)

<img src="screenshots\Practica 2\ej14-3.jpg"/>

Cuarto acceso (Agrego header `Authorization` con Base64)

<img src="screenshots\Practica 2\ej14-4.jpg"/>

Quinto acceso (Encuentro la siguiente página en los headers de la respuesta)

<img src="screenshots\Practica 2\ej14-5.jpg"/>

Último acceso

<img src="screenshots\Practica 2\ej14-6.jpg"/>

# 15) Utilizando la VM, realice la siguientes pruebas:

## a. Ejecute el comando `curl www.redes.unlp.edu.ar/extras/prueba-http-1-0.txt` y copie la salida completa (incluyendo los dos saltos de linea del final).

<img src='./screenshots/Practica 2/ej15.jpg'>

## b. Desde la consola ejecute el comando `telnet www.redes.unlp.edu.ar 80` y luego pegue el contenido que tiene almacenado en el portapapeles. ¿Qué ocurre luego de hacerlo?

Recibe una respuesta del servidor con los encabezados:


| HTTP/1.1 200 OK |
| --- |
**Date:** Sun, 28 Aug 2022 17:32:23 GMT
**Server:** Apache/2.4.53 (Unix)
**Last-Modified:** Wed, 13 Apr 2022 22:55:32 GMT
**ETag:** "760-5dc9113140100"
**Accept-Ranges:** bytes
**Content-Length:** 1888
**Connection:** close
**Content-Type:** text/html

Y la respuesta es un archivo HTML completo muy grande así que no lo voy a subir todo pero lo importante es que dice

    <h1>Ejemplo del protocolo HTTP 1.1</h1>
    <p>
        Esta página se visualiza utilizando HTTP 1.1. Utilizando el capturador de paquetes analice cuantos flujos utiliza el navegador para visualizar la página con sus imágenes en contraposición con el protocolo HTTP/1.0.
    </p>
    </p>
    <h2>Imagen de ejemplo</h2>
    <img src="13532-tuxkiller03green.png" width="800px"/>
    </div> 
    
    
    </div>
    <div id="footer">
      <div class="container">
        <p class="muted credit">Redes y Comunicaciones</p>
      </div>
    </div>

## c. Repita el proceso anterior, pero copiando la salida del recurso `/extras/prueba-http-1-1.txt`. Verifique que debería poder pegar varias veces el mismo contenido sin tener que ejecutar telnet nuevamente.

El recurso retorna:

<img src='./screenshots/Practica 2/ej15c.jpg'>

Al ejecutar `telnet` la respuesta es la misma, pero permite copiar varias veces el contenido (y enviarlo) hasta que se cierra la conexión.

# 16) En base a lo obtenido en el ejercicio anterior, responda:

## a. ¿Qué está haciendo al ejecutar el comando `telnet`?
El comando telnet es una herramienta que sirve para emular una terminal manejada por el protocolo TELNET. Permite el envío y recepción de datos de un servidor remoto usando este protocolo.

## b. ¿Qué método HTTP utilizó?  ¿Qué recurso solicitó?
En ambos casos usó el método GET y recibió como recurso el archivo HTTP de la página Web.

## c. ¿Qué diferencias notó entre los dos casos? ¿Puede explicar por qué?
La única diferencia es que cuando se establece la conexión de HTTP 1.0 se cierra automáticamente, mientras que al conectarse con HTTP 1.1 la conexión persiste un tiempo más y permite volver a enviarle contenido mientras dure.

Esto se debe a que en HTTP 1.1 se implementó una característica que implementa conexiones persistentes para hacer múltiples requests. En la versión anterior (HTTP 1.0) las conexiones se cerraban instantáneamente.

## d. ¿Cuál de los dos casos le parece más eficiente? Piense en el ejercicio donde analizó la cantidad de requerimientos necesarios para obtener una página con estilos, javascripts e imágenes. El caso elegido, ¿puede traer asociado algún problema?
El problema de HTTP 1.0 es que un cliente no podía realizar peticiones consecutivas, por lo que debía esperar a poder volver conectarse con el servidor. Esto producía problemas de eficiencia, especialmente al intentar cargas páginas Web con varios recursos (como la del ejercicio); si el servidor tuviese muchas solicitudes que atender, podría demorarse mucho en volver a atender a la del cliente.

Gracias a las conexiones persistentes de HTTP 1.1, cuando un cliente se conecta con el servidor puede realizar peticiones adicionales inmediatamente, dentro de la misma conexión. Esto permite que obtenga todos los recursos adicionales que necesita, sin liberar al servidor y tener que reconectarse con él.

# 17) En el siguiente ejercicio veremos la diferencia entre los métodos POST y GET. Para ello, será necesario utilizar la VM y la herramienta Wireshark.

## a. Abra un navegador e ingrese a la URL: www.redes.unlp.edu.ar e ingrese al link en la sección “Capa de Aplicación” llamado “Métodos HTTP”. En la página mostrada se visualizan dos nuevos links llamados: Método GET y Método POST.

## b. Analice el código HTML.
Si inspeccionamos el HTML de ambas páginas vemos que los formularios son prácticamente iguales; lo único que cambia es el método (GET y POST respectivamente). Incluso ambos apuntan hacia la misma acción `metodos-lectura-valores.php`, aunque el servidor los va a tratar de distinta manera puesto que tienen distinto método.

## c. Utilizando el analizador de paquetes Wireshark capture los paquetes enviados y recibidos al presionar el botón Enviar.

### En el método GET:
**Petición:**

<img src='./screenshots/Practica 2/ej17c-get1.jpg'>

Los valores de los campos del formulario no están en el cuerpo de la petición, sino que aparecen en la URL: 

`form_nombre=Joaqu%C3%ADn&form_apellido=Spinelli&
form_mail=email_prueba%40gmail.com&form_sexo=sexo_masc& form_pass=12345`

**Respuesta:**

<img src='./screenshots/Practica 2/ej17c-get2.jpg'>

Y en el cuerpo de la respuesta está el archivo HTML.


### En el método POST:
**Petición:**

<img src='./screenshots/Practica 2/ej17c-post1.jpg'>

Los valores de los campos del formulario se encuentran en el cuerpo de la petición, en un apartado especial para formularios HTML

<img src='./screenshots/Practica 2/ej17c-post2.jpg'>

**Respuesta:**

<img src='./screenshots/Practica 2/ej17c-post3.jpg'>

La respuesta es la misma que con el método GET (también devuelve la página HTML en el cuerpo)

# 18) Investigue cuál es el principal uso que se le da a las cabeceras Set-Cookie y Cookie en HTTP y qué relación tienen con el funcionamiento del protocolo HTTP.

La cabecera `Set-Cookie` se usa para enviar cookies desde el servidor al User Agent, para que pueda agregarlas como encabezado en futuros mensajes al servidor.

La cabecera `Cookie` contiene los cookies que el User Agent desea enviar al servidor en un mensaje.

Ambas funcionan en conjunto, puesto que el User Agent recibe las cookies mediante `Set-Cookie` y luego puede enviarlas al servidor mediante `Cookie`.

En HTTP las cookies son datos que el cliente recibe del servidor y luego las agrega en las nuevas peticiones que envía. Pueden usarse para múltiples propósitos, pero los más comunes son para gestionar sesiones de usuario, personalizar configuraciones o rastrear comportamientos de usuario.

Como HTTP es un protocolo sin estado (es decir, no guarda información de comunicaciones previas), las cookies permiten que el servidor "recuerde" información del estado del cliente con el que se está comunicando. Esto permite, por ejemplo, mantener abiertas las sesiones de usuario o cargar una página Web con las preferencias que seleccionó previamente, incluso después de haber cerrado la página o el navegador.

# 19) ¿Cuál es la diferencia entre un protocolo binario y uno basado en texto? ¿de que tipo de protocolo se trata HTTP/1.0, HTTP/1.1 y HTTP/2?
Los protocolos en binarios utilizan un conjunto estandarizado de caracteres de control para enviar datos codificados en binario en una comunicación. Están pensados para ser leídos por máquinas, por lo que cada una debe tener herramientas para interpretarlos.

Los protocolos basados en texto usan caracteres ASCII y están formados por cadenas de texto pensadas en un formato para ser legibles por humanos. Debido a este formato legible, las máquinas suelen tener problemas para intepretarlos, tales como los espacios en blanco, capitalización, caracteres de final de línea, etc.

Los protocolos binarios son más eficientes para interpretar, más compactos al ser transportados, y son mucho menos propenso a errores.

Los protocolos basado en texto son usados por HTTP/1.0 y HTTP/1.1, mientras que HTTP/2 usa protocolo binario.

# 20) Analice de que se tratan las siguientes características de HTTP/2: stream, frame, server-push

- **Frame:** unidad mínima de comunicación. Contienen un header que identifican al stream al cual pertenecen. Llevan tipos de datos específicos, tales como headers, carga útil, etc.
- **Stream:** secuencias de frames independientes y bidireccionales, que se intercambian entre el cliente y el servidor a lo largo de una conexión. Una única conexión puede tener varios streams activos, y el nodo destino los procesará en el orden de envío.
- **Server-push:** es un servicio basado en estimaciones y predicciones. Sirve para que el servidor envíe información al usuario antes de que él la solicite, para que cuando lo haga esté disponible inmediatamente. Funciona enviando múltiples respuestas a una única petición.

# 21) Responder las siguientes preguntas:
## a. ¿Qué función cumple la cabecera Host en HTTP 1.1? ¿Existía en HTTP 1.0? ¿Qué sucede en HTTP/2?
El header `Host` se usa para especificar el nombre del dominio al que el cliente quiere hacer la petición. Opcionalmente puede agregarse el número de puerto al que se referencia.

En conexiones HTTP 1.0 no era requerido, pero podía agregarse opcionalmente; en HTTP 1.1 es obligatorio que todas las comunicaciones lo tengan especificado.

En HTTP/2 se reemplazó por el header `Authority`, que cumple una función similar.

También se agregaron algunos pseudo-headers:
- `Method`: Método de la petición.
- `Path`: Path al que se realiza la peticicón.
- `Scheme`: el esquema al que se realiza la petición (HTTP o HTTPS)

## b. ¿Cómo quedaría en HTTP/2 el siguiente pedido realizado en HTTP/1.1 si se está usando https?

```html
GET /index.php HTTP/1.1
Host: www.info.unlp.edu.ar
```

```html
:method | GET
:scheme | https
:authority | www.info.unlp.edu.ar
:path | index.php
```