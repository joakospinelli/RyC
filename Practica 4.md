# 1) ¿Qué protocolos se utilizan para el envío de mails entre el cliente y su servidor de correo? ¿Y entre servidores de correo?

El usuario establece la conexión con el servidor de correo a través de un `MUA` (Mail User Agent), que se conecta con el servidor con el protocolo SMTP. Para comunicarse entre servidores se usa el mismo protocolo.

SMTP es un protocolo en línea usado para el intercambio de emails. En este protocolo, el MUA envía el correo al `MSA` (Mail Submission Agent) y finalmente este lo envía al `MTA` (Mail Transfer Agent). Luego, el MTA debe enviar los correos al receptor; para esto usa los registros MX (DNS) que encuentra en el dominio del email receptor.
Una vez que establece la conexión a los servidores de email, son recibidos por el `MDA` (Mail Delivery Agent), que realiza la entrega local del correo.

Este protocolo está limitado para la recuperación y recepción de emails, por lo que se debe acompañar con otros protocolos para esa función (comunmente POP3 o IMAP). Además, es un protocolo sin autenticación, por lo que podrían enviarse mails con un correo emisor sin ser el propietario de esa cuenta.

# 2) ¿Qué protocolos se utilizan para la recepción de mails? Enumere y explique características y diferencias entre las alternativas posibles.

Para la recepción de emails se usan más comunmente los protocolos POP3 o IMAP. A diferencia de SMTP, estos protocolos son más seguros, requieren autenticación y se comunican directamente con el cliente (a diferencia de SMTP que requiere conectarse desde el MUA al MSA, y luego al MTA). Las diferencias entre estos protocolos son:

* `POP3`: es un protocolo más simple, con funcionalidad limitada. Cuando un usuario se conecta, se descargan todos los emails recibidos.

  Para recibir los correos, el cliente establece una conexión TCP con el servidor de correo (en el puerto 110), y cumple 3 fases:
    * **Autorización**: el User Agent envía las credenciales de la cuenta.
    * **Transacción**: una vez que se autenticó el usuario, se pueden manipular los mensajes usando las instrucciones del protocolo (_LIST_ para listar mensajes, _RETR_ para ver un mensaje concreto y _DELE_ para eliminar un mensaje); si el usuario quiere eliminar mensajes, estos no se eliminan directamente sino que quedan marcados.
    * **Actualización**: cuando el usuario terminó de realizar todas sus operaciones y usa el comando `QUIT`, se borran los datos de sesión y se eliminan los mensajes que hayan sido marcados.

* `IMAP`: es un protocolo más avanzado que presenta bastantes ventajas, sin embargo es más complejo. Permite la visualización de los emails desde el servidor, y sólo descarga los que el usuario finalmente decida leer (a diferencia de POP3 que descarga todos). Los archivos de correo está nconstantemente sincronizados con el servidor, por lo que se puede acceder en varios dispositivos a la vez y ver información consistente. También permite la organización de mensajes en carpetas o plantillas directamente en el servidor, por lo que pueden verse en distintos dispositivos.

# 3) Utilizando la VM y teniendo en cuenta los siguientes datos, abra el cliente de correo (thunderbird) y configure dos cuentas de correo. Una de las cuentas utilizará POP para solicitar al servidor los mails recibidos para la misma mientras que la otra utilizará IMAP.

<img src="./screenshots/Practica 4/ej3.png">

## a. Verificar el correcto funcionamiento enviando un email desde el cliente de una cuenta a la otra y luego desde la otra responder el mail hacia la primera.

<img src="./screenshots/Practica 4/ej3a-1.png">

<img src="./screenshots/Practica 4/ej3a-2.png">

## b. Análisis del protocolo SMTP

### i. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta `alumnopop@redes.unlp.edu.ar` envía un correo a `alumnoimap@redes.unlp.edu.ar`.

(Acá no sé como capturar **exactamente** el tráfico al servidor de correo o si con capturar en el de siempre está bien)

<img src="./screenshots/Practica 4/ej3b-1.png">

Podemos ver que captura conexiones de los protocolos DNS, TCP, SMTP y TLS.

### ii. Utilice el filtro SMTP para observar los paquetes del protocolo SMTP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta. Ayuda: filtre por protocolo SMTP y sobre alguna de las líneas del intercambio haga click derecho y seleccione Follow TCP Stream

<img src="./screenshots/Practica 4/ej3b-3.png">

<img src="./screenshots/Practica 4/ej3b-2.png">

## c. Usando el cliente de correo, thunderbird del usuario `alumnopop@redes.unlp.edu.ar` envíe un correo electrónico `alumnoimap@redes.unlp.edu.ar` el cual debe tener: un asunto, datos en el body y una imagen adjunta.

<img src="./screenshots/Practica 4/ej3c-1.png">

(Abajo está la imagen codificada en Base64 pero es mucho texto y la imagen me va a quedar larguísima)

### i. Verifique los fuentes del correo recibido para entender como se utiliza el header `Content-Type: multipart/mixed` para poder realizar el envío de distintos archivos adjuntos.

El valor `multipart/mixed` en el encabezado Content-Type indica que el mensaje va a recibir apartados con distintos tipos de contenido, y que por lo tanto el cliente va a tener que decodificarlos de manera distinta. 

También vemos que se le agrega el campo `boundary`; este campo se usa para identificar a un divisor entre los distintos tipos de contenido del email. 

Debajo de cada divisor vemos nuevamente el encabezado `Content-Type` y  otro llamado `Content-Transfer-Encoding`. Estos fueron definidos por el estándar MIME, e indican respectivamente el tipo del contenido y el algoritmo usado para codificarlo y decodificarlo. Estos encabezados van a ser usados por el cliente para saber cómo debe renderizar el email.

En este ejemplo, se usó el formato MIME para:
* Cuerpo del email: `Content-Type: text` y `Content-Transfer-Encoding: 7bit`
* Imagen adjunta: `Content-Type: image/jpeg` y `Content-Transfer-Type: base64`. También se le agregó `Content-Disposition: attachment` para identificarlo como archivo adjunto.

### ii. Extraiga la imagen adjunta del mismo modo que lo hace el cliente de correo a partir de los fuentes del mensaje.

Acá no sé a qué se refiere exactamente, pero encontré una página para decodificar una imagen a partir de un texto codificado en Base64 (https://codebeautify.org/base64-to-image-converter).

<img src="./screenshots/Practica 4/ej3c-2.png">

# 4) Análisis del protocolo POP

## a. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta `alumnoimap@redes.unlp.edu.ar` le envía una correo a `alumnopop@redes.unlp.edu.ar` y mientras `alumnopop@redes.unlp.edu.ar` recepciona dicho correo.

Envío del mail:

<img src="./screenshots/Practica 4/ej4a-1.png">

Recepción del mail:

<img src="./screenshots/Practica 4/ej4a-2.png">

## b. Utilice el filtro POP para observar los paquetes del protocolo POP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta.

<img src="./screenshots/Practica 4/ej4b.png">

Para recibir los mensajes en POP hay que hacer clic en el botón de "Get Messages". Los mensajes que su campo _INFO_ empiece con **C:** son enviados por el cliente y contienen las comandos o envían información solicitada por el servidor; los que empiezan con **s:** son enviados por el servidor y contienen las respuestas a los comandos.

Los comandos de POP3 mostrados en la captura son:

| Comando | Descripción |
| --- | --- |
| **CAPA** | El servidor retorna _+OK_ si acepta el comando `UIDL`. |
| **AUTH** | Indica al servidor que se va a iniciar el proceso de autenticación para la sesión; en el primer parámetro se le pasa el tipo de autenticación (en este caso, _PLAIN_). Si el servidor responde con _+_ entonces el cliente puede enviar la información de autenticación. |
| **STAT** |  Solicita información sobre la bandeja de entrada. En la respuesta del servidor se incluye la cantidad de correos y el tamaño total en bytes (en este caso, 1 correo y 766 bytes). |
| **LIST** | Solicita información sobre los mensajes y el tamaño individual de cada uno. En este caso, devuelve que hay sólo un mensaje (si abro la entrada de la captura se ve también el tamaño). |
| **UIDL** | Solicita información sobre un mensaje en concreto. El servidor retorna un identificador único para este mensaje (igual en la captura que saqué no lo usa así 😐 Capaz porque había un sólo mail). |
| **RETR** | Solicita la recuperación de un correo en particular (pasado como parámetro). El servidor retorna el correo y el cliente _debería_ descargarlo tras recibirlo. |
| **QUIT** | solicita la terminación de la sesión POP3. Si había mensajes marcados para ser eliminados, el servidor los elimina al recibir este comando. |

# 5) Análisis del protocolo IMAP
## a. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta `alumnopop@redes.unlp.edu.ar` le envía una correo a `alumnoimap@redes.unlp.edu.ar` y mientras `alumnoimap@redes.unlp.edu.ar` recepciona dicho correo.

Envío del mail (Esta captura se entiende mejor que la otra pero no creo que sea por el IMAP):

<img src="./screenshots/Practica 4/ej5a-1.png">

Recepción del mail:

<img src="./screenshots/Practica 4/ej5a-2.png">

## b. Utilice el filtro IMAP para observar los paquetes del protocolo IMAP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta.

<img src="./screenshots/Practica 4/ej5b.png">

A diferencia de POP3, los correos en IMAP se reciben automáticamente. Los mensajes que su campo _INFO_ empiece con **Request:** son enviados por el cliente y contienen las comandos o envían información solicitada por el servidor; los que empiezan con **Response:** son enviados por el servidor y contienen las respuestas a los comandos.

Los comandos usados son:
| Comando | Descripción |
| --- | --- |
|**Authenticate** | Indica al servidor que el cliente va a autenticarse; se le pasa como parámetro el tipo de autenticación (en este caso PLAIN). Si el servidor responde con _+_, entonces el cliente puede enviar los datos de su cuenta; luego el cliente va a responder con _OK_ si la autenticación fue correcta. |
| **ID** | Le indica al servidor información sobre el cliente que está intentando conectarse con él (en este caso, _Thunderbird_). |
| **ENABLE** | Solicita al servidor que habilite ciertas extensiones para el intercambio de emails (en este caso, solicita que se habilite la codificación UTF-8). |
| **SELECT** | Solicita al servidor que se selecciona una carpeta en específico (en este caso, _INBOX_). Luego todas las operaciones sobre carpetas se realizarán sobre la seleccionada. |
| **FETCH** | Busca un mensaje específico en la carpeta seleccionada. |
| **IDLE** | Le indica al servidor que el cliente está listo para recibir actualizaciones de la bandeja de entrada en tiempo real, sin tener que consultarle continuamente. |
| **NOOP** | Solicita al servidor que se mantenga abierta la conexión IMAP, para que no se cierre automáticamente. |

# 6) IMAP vs POP3
## a. Marque como leídos todos los correos que tenga en el buzón de entrada de `alumnopop` y de `alumnoimap`. Luego, cree una carpeta llamada POP en la cuenta de `alumnopop` y una llamada IMAP en la cuenta de `alumnoimap`. Asegurese que tiene mails en el inbox y en la carpeta recientemente creada en cada una de las cuentas.

<img src="./screenshots/Practica 4/ej6a.png">

## b. Cierre la sesión iniciada e ingrese nuevamente identificandose como usuario root y password packer, ejecute el cliente de correos. De esta forma, iniciará el cliente de correo con el perfil del superusuario (diferente del usuario con el que ya configuró las cuentas antes mencionadas). Luego configure las cuentas POP e IMAP de los usuarios alumnopop y alumnoimap como se describió anteriormente pero desde el cliente de correos ejecutado con el usuario root.


<img src="./screenshots/Practica 4/ej6b-1.png">

(ACÁ ME TIRA ESTE ERROR NO SÉ POR QUÉ. COMO NO ME DEJÓ CREAR LA CARPETA DE IMAP VOY A CONTESTAR SUPONIENDO QUE PUDE HACER TODO 😎😎😎)

### i. ¿Qué correos ve en el buzón de entrada de ambas cuentas? ¿Están marcados como leídos o como no leídos? ¿Por qué?

Los correos de la cuenta `alumnopop` están marcados como no leídos, mientras que en `alumnosimap` aparecen ya leídos. Esto se debe a que el protocolo POP3 realiza las lecturas de mensajes en el cliente, por lo que los mensajes leídos o no leídos se guardan en el dispositivo en el que se leyeron. Por otro lado, IMAP permite sincronizar las lecturas con el servidor, por lo que los mensajes leídos quedan marcados directamente en el servidor, y el cliente puede verlos desde cualquier dispositivo en el que ingrese.

### ii. ¿Qué pasó con las carpetas POP e IMAP que creó en el paso anterior?

La carpeta de "POP" no aparece, puesto que este protocolo no permite crear carpetas en el servidor. La carpeta en realidad se creó y almacenó localmente.

La carpeta de "IMAP" es la que me dio error para crearla, pero si la hubiese hecho bien __en teoría__ debería aparecer, porque este protocolo sí permite organizar la bandeja de entrada por carpetas en el servidor; entonces, cuando recibo los mails los recibo en las carpetas.

## c.  En base a lo observado. ¿Qué protocolo le parece mejor? ¿POP o IMAP? ¿Por qué? ¿Qué protocolo considera que utiliza más recursos del servidor? ¿Por qué?

IMAP es un protocolo mucho más completo, que facilita la lectura y organización de los correos para el usuario, además de que puede acceder a su cuenta desde varios dispositivos y mantenerse sincronizado.

También es el que utiliza más recursos del servidor, puesto que toda la información del usuario para la organización de la bandeja de entrada se almacena directamente en el servidor de correo, a diferencia de POP que descarga toda la bandeja de entrada localmente.

# 7) ¿En algún caso es posible enviar más de un correo durante una misma conexión TCP?

# 8) Indique sí es posible que el MSA escuche en un puerto TCP diferente a los convencionales y qué implicancias tendría.

# 9) Indique sí es posible que el MTA escuche en un puerto TCP diferente a los convencionales y qué implicancias tendría.

# 10) Ejercicio integrador HTTP, DNS y MAIL

(ES RE LARGO ESTE NO LO VOY A HACER 😴😴😴)

# 11) Utilizando la herramienta Swaks envíe un correo electrónico

`swaks` es un comando que permite enviar correos electrónicos desde la terminal.

<img src="./screenshots/Practica 4/ej11.png">

## a. Analice tanto la salida del comando swaks como los fuentes del mensaje recibido para responder las siguientes preguntas

Salida del comando `swaks`:

<img src="./screenshots/Practica 4/ej11a-1.png">

<img src="./screenshots/Practica 4/ej11a-2.png">

Fuente del mensaje recibido:

<img src="./screenshots/Practica 4/ej11a-3.png">

### i. ¿A qué corresponde la información enviada por el servidor destino como respuesta al comando EHLO? Elija dos de las opciones del listado e investigue la funcionalidad de la misma.

El comando `HELO` es usado por el cliente para iniciar la sesión de intercambio de correos. Se le agrega un argumento con el nombre del ordenador (en este caso, `debian`).

En este caso en particular se usa `EHLO`, que es la alternativa para el ESMTP (Enhanced SMTP); cumple la misma función que el `HELO` del SMTP tradicional, pero además el servidor va a responder si soporta o no ESMTP; si lo hace, también va a devolver la lista de los comandos de la extensión que soporta.

* `STARTTLS`: le indica al servidor que el cliente quiere iniciar una transferencia en modo seguro, a través del protocolo TLS. Si el servidor lo acepta, el protocolo va a volver a su estado inicial, por lo que el cliente va a tener que iniciar la sesión de nuevo con el comando `HELO/EHLO`.
* `PIPELINING`: le indica al servidor que la conexión con el clienteva a realizarse en modo Pipelining. Esto significa que el cliente va a poder enviar varios mensajes seguidos al servidor sin esperar la respuesta de los anteriores.

### ii. Indicar cuáles cabeceras fueron agregadas por la herramienta swaks.

Los headers que agregó fueron:
* **Subject**: este lo añadimos en el comando para enviar el correo, con la opción `--h-Subject`.
* **X-Mailer**: es usado para indicar qué herramienta se utilizó para enviar el correo. En este caso, lo agregó indicando que el mail se envió a través del comando `swaks`.
* **Content-Type**: este se agregó porque adjuntamos un archivo, por lo que indica que que el mail es multipartes, con contenido variado; también define el divisor del MIME.

### iii. ¿Cuál es el message-id del correo enviado? ¿Quién asigna dicho valor?

El Message-ID es `<20220916184021.003296@debian>`, que está formado por la fecha y hora de envío del correo más el nombre del cliente (el que mandamos con `EHLO`).

Comunmente los Message-ID son generados por el cliente que envía el mail o por el primer servidor de correos por el que pasan.

### iv.  ¿Cuál es el software utilizado como servidor de correo electrónico?

El software utilizado es _Postfix_. Esto lo vemos en el apartado _"by"_ del header `Received`, en el que aparece el servidor que recibió el mail (`mail.redes.unlp.edu.ar`) y entre paréntesis el Software.

## b. Descargue de la plataforma la captura de tráfico smtp.pcap y la salida del comando swaks smtp.swaks

### i. ¿Por qué el contenido del mail no puede ser leido en la captura de tráfico?

Porque al comienzo de la sesión SMTP el cliente usó el comando `STARTTLS`, indicando que la comunicación va a realizarse mediante TLS. Este es un protocolo para intercambios seguros, por lo que los mensajes se encuentran encriptados de manera que no se lean.

En la captura de tráfico podemos ver que, luego de que el cliente recibe la aprobación del comando `STARTTLS`, deja de haber intercambios en el protocolo SMTP, y todos pasan a ser en TLS.

<img src="./screenshots/Practica 4/ej11b.png">

En los mensajes del protocolo TLS que tienen en Info _"Application Data"_ podemos ver que especifica que el protocolo de los datos de la aplicación es SMTP, pero que están encriptados.

## c. Realice una consulta de DNS por registros TXT al dominio info.unlp.edu.ar y entre dichos registros evalúe la información del registro SPF. ¿Por qué cree que aparecen muchos servidores autorizados?

<img src="./screenshots/Practica 4/ej11c.png">

Entre los registros vemos que hay uno que indica SPF, con el contenido: "`v=spf1 mx a:mail3.info.unlp.edu.ar a:listas.extension.info.unlp.edu.ar a:mail-app.info.unlp.edu.ar a:biblioteca.info.unlp.edu.ar a:catedras.info.unlp.edu.ar a:moodle.linti.unlp.edu.ar ~all`".

El registro SPF es usado en DNS para indicar los servidores autorizados para enviar correos desde un dominio concreto; en este caso, desde _info.unlp.edu.ar_.  Como el protocolo SMTP no acepta autenticación, el registro SPF se usa como una manera de asegurar la veracidad del remitente de un correo.

Todos los servidores de correo que aparecen son los que usan las autoridades de la facultad, por lo que todos deben estar autorizados para el envío de emails.

## d. Realice una consulta de DNS por registros TXT al dominio outlook.com y analice el registro correspondiente a SPF. ¿Cuáles son los bloques de red autorizados para enviar mails?. Investigue para qué se utiliza la directiva "~all"

<img src="./screenshots/Practica 4/ej11d.png">

En este registro SPF se hace una diferencia entre la IPv4 autorizada y los servidores que tienen la etiqueta `include`.

La dirección IPv4 (en este caso, _157.55.9.128/25_) está autorizada para enviar correos electrónicos en nombre del dominio.

Los servidores que tienen antes la etiqueta `include` son servidores de terceros que están autorizados para enviar correos electrónicos en nombre del dominio. Esto también incluye a las direcciones IP contenidas por dicho servidor. En este registro, los servidores de tercerso autorizados son:
* `spf-a.outlook.com`
* `spf-b.outlook.com`
* `spf.protection.outlook.com`
* `spf-a.hotmail.com`
* `spf-ssg-b.microsoft.com`
* `spf-ssg-c.microsoft.com`

Por último, la opción `~all` indica que los correos electrónicos que provengan de dominios o direcciones IP no autorizadas serán aceptadas, pero marcadas como inseguras o no deseadas. Las alternativas a esta opción son:
* `+all`: cualquier servidor no incluido puede enviar correos electrónicos en nombre del dominio.
* `-all`: los correos electrónicos de direcciones IP o dominios no autorizados son rechazados y eliminados.

# 12) Observar el gráfico a continuación y teniendo en cuenta lo siguiente, responder:

<img src="./screenshots/Practica 4/ej12.png">

* El usuario `juan@misitio.com.ar` en PC-A desea enviar un mail al usuario `alicia@example.com`
* Cada organización tiene su propios servidores de DNS y Mail
* El servidor ns1 de `misitio.com.ar` no tiene la recursión habilitada

## a. El servidor de mail, mail1, y de HTTP, www, de example.com tienen la misma IP, ¿Es posible esto? Si lo es, ¿cómo lo resolvería?

Es posible que esto pase, puesto que los protocolos de Mail y HTTP usan puertos distintos (SMTP usa el 25, HTTP usa el 80 o el 443 para HTTPS). Esto permite que ambos usen el mismo servidor (con la misma IP), pero realicen las comunicaciones en puertos separados.

Cuando un cliente se conecta a un servidor, debe especificarle al protocolo de la capa de transporte la dirección IP y el puerto del destino (entre otras cosas). De esta manera, el servidor va a recibir las comunicaciones de ese protocolo en dicho puerto, sin interferir con otros servicios que estén esperando en otros.

## b. Al enviar el mail, ¿por qué registro de DNS consultará el MUA?

El MUA va a consultar por el registro MX del servidor de mail del emisor; en este caso, por el servidor `smtp-5`.

## c. Una vez que el mail fue recibido por el servidor smtp-5, ¿por qué registro de DNS consultará?

El servidor `smtp-5` tiene que enviar el correo al servidor receptor del mail. Para esto va a hacer una consulta a los registros MX del dominio `example.com`.

## d. Si en el punto anterior smtp-5 recibiese un listado de nombres de servidores de correo, ¿será necesario realizar una consulta de DNS adicional? Si es afirmativo, ¿por qué tipo de registro y de cuál servidor preguntaría?

Si recibiese un listado de servidores, probablemente se deba a que haya varios y que estén ordenados según el número de prioridad. En este caso, `smtp-5` va a tener que preguntar por el registro A del servidor con mayor prioridad (el menor número).

## e. Indicar todo el proceso que deberá realizar el servidor ns1 de misitio.com.ar para obtener los servidores de mail de example.com
(ESTO NO ESTOY SEGURO SI ESTÁ BIEN PORQUE NS1 TIENE LA RECURSIÓN DESHABILITADA)
* `ns1` realiza una consulta iterativa por los registros A a `root-servers` para obtener la dirección IP de `gtld-servers`.
* `ns1` obtiene la dirección de `gtld-servers` y le realiza una consulta iterativa por los registros A para obtener la dirección IP de `mail1`.
* `ns1` obtiene la dirección de `mail1` y le realiza una consulta iterativa para obtener sus registros MX.

## f. Teniendo en cuenta el proceso de encapsulación/desencapsulación y definición de protocolos, responder V o F y justificar:

### Los datos de la cabecera de SMTP deben ser analizados por el servidor DNS para responder a la consulta de los registros MX

### Al ser recibidos por el servidor smtp-5 los datos agregados por el protocolo SMTP serán analizados por cada una de las capas inferiores

Verdadero. En el modelo TCP/IP, para establecer las comunicaciones correctamente cada capa agrega y modifica los datos de las capas superiores. Cuando dichos datos llegan al destino, se realiza el proceso inverso en cada una de las capas.

### Cada protocolo de la capa de aplicación agregará una cabecera con información propia de ese protocolo

### Como son todos protocolos de la capa de aplicación, las cabeceras agregadas por el protocolo de DNS puede ser analizadas y comprendidas por el protocolo SMTP o HTTP

### Para que los cliente en misitio.com.ar puedan acceder el servidor HTTP www.example.com y mostrar correctamente su contenido deben tener el mismo sistema operativo

Falso. Una de las características del protocolo HTTP (y de la mayoría de protocolos definidos en RFC) es que es independiente del Sistema Operativo del cliente o del servidor; el contenido va a transmitirse de la misma manera en todos los SO siempre que soporten el protocolo deseado.

### Un cliente web que desea acceder al servidor www.example.com y que no pertenece a ninguno de estos dos dominios puede usar a ns1 de misitio.com.ar como servidor de DNS para resolver la consulta.

(ESTA ES LA 12.G PERO CREO QUE LES QUEDÓ MAL LA LISTA Y EN REALIDAD ERA DEL VERDADERO O FALSO)

Falso. `ns1` se usa para resolver las consultas referidas a su propio servidor. Además, como no es autoritativo de ningún dominio relacionado a `www.example.com` no tiene sentido que le haga consultas para resolverlo.

## h. Cuando Alicia quiera ver sus mails desde PC-D, ¿qué registro de DNS deberá consultarse?

Hice la prueba en Wireshark y primero consulta por el SOA y después por A, pero no sé por qué ni si está bien. SUPONGO que no consulta por MX porque esos se usan para intercambiar mails entre servidores distintos, no para recibir los mails desde el mismo servidor.

<img src="./screenshots/Practica 4/ej12h.png">

## i. Indicar todos los protocolos de mail involucrados, puerto y si usan TCP o UDP, en el envío y recepción de dicho mail.

Los protocolos de mail usados van a ser:
* `SMTP`: es usado para el envío del mail. Usa el puerto 25. En la capa de transporte usa TCP.
* `POP3`: es usado para la recepción del mail (puede ser este o IMAP). Usa el puerto 110. En la capa de transporte usa TCP.
* `IMAP`: es usado para la recepción del mail (puede ser este o POP3). Usa el puerto 143. En la capa de transporte usa TCP.

