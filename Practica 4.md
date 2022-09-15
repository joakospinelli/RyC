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

??????????????????????????????

### ii. ¿Qué pasó con las carpetas POP e IMAP que creó en el paso anterior?

La carpeta de "POP" no aparece, puesto que este protocolo no permite crear carpetas en el servidor. La carpeta en realidad se creó y almacenó localmente.

La carpeta de "IMAP" es la que me dio error para crearla, pero si la hubiese hecho bien __en teoría__ debería aparecer, porque este protocolo sí permite organizar la bandeja de entrada por carpetas en el servidor; entonces, cuando recibo los mails los recibo en las carpetas.

## c.  En base a lo observado. ¿Qué protocolo le parece mejor? ¿POP o IMAP? ¿Por qué? ¿Qué protocolo considera que utiliza más recursos del servidor? ¿Por qué?

IMAP es un protocolo mucho más completo, que facilita la lectura y organización de los correos para el usuario, además de que puede acceder a su cuenta desde varios dispositivos y mantenerse sincronizado.

También es el que utiliza más recursos del servidor, puesto que toda la información del usuario para la organización de la bandeja de entrada se almacena directamente en el servidor de correo, a diferencia de POP que descarga toda la bandeja de entrada localmente.
