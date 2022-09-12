# 1) Investigue y describa cómo funciona el DNS. ¿Cuál es su objetivo?
DNS es la sigla para Domain Name System (sistema de nombres de dominio). Su objetivo es el de asignarle a las direcciones IP de los servidores un nombre que sea legible y recordable para los usuarios, para que estos puedan conectarse cómodamente. Es un protocolo presente en la capa de aplicación, que se maneja con TCP/UDP en la capa de transporte (en el puerto 53).

El servidor DNS usa una base de datos distribuida y jerárquica que contiene los nombres de dominio de la red. Los servidores se buscan en la BD a partir del nombre de dominio ingresado de manera jerárquica, según el nivel de las etiquetas que forman al dominio completo.

# 2)  ¿Qué es un root server? ¿Qué es un generic top-level domain (gTLD)?
Los root servers son los primeros servidores en realizar la traducción de los nombres de dominio de DNS a direcciones IP. Es el servidor de mayor nivel, y se encarga de subdelegar las consultas al servidor de alto nivel más cercano que esté autorizado para resolver ese dominio. Actualmente hay 13 root servers distribuidos por todo el mundo.

Los GTLD son los dominios de nivel superior genéricos. Es una categoría que se le dio a los TLD de uso común, aunque anteriormente cada uno tenía un propósito común.

Pueden dividirse en:
* **Unsponsored TLD:** sus políticas de uso están definidas por la ICANN. Su uso es libre para cualquiera, y para cualquier propósito. Entre ellos se encuentran `.com`, `.net` o `.org`.
* **Sponsored TLD:** sus políticas de uso están definidas por otras organizaciones. Su uso tiene un propósito específico, generalmente destinado para las actividades de las organizaciones que los definen. Entre ellos se encuentran `.edu` o `.gob`. 

# 3) ¿Qué es una respuesta del tipo autoritativa?
Una respuesta autoritativa indica que la consulta DNS Fue respondida directamentepor el servidor autoritativo del nombre de dominio que estamos buscando, sin que éste subdelegue a otro.

Los servidores autorativos son aquellos que están a cargo de un conjunto de nombres o subdominios.

# 4) ¿Qué diferencia una consulta DNS recursiva de una iterativa?
En una consulta iterativa, cuando el cliente hace una solicitud a un servidor DNS se le devuelve la dirección IP al TLD de ese dominio. Con esta dirección, el cliente debe volver a consultar al servidor nuevo, y así sucesivamente hasta obtener la dirección IP del dominio completa.

En una consulta recursiva, el cliente espera recibir la dirección IP completa en la primer solicitud al servidor DNS.

# 5) ¿Qué es el resolver?
El resolver actúa como intermediario entre el cliente y un servidor DNS. Contiene una caché con las direcciones IP solicitadas recientemente, para entregárselas directamente al cliente en el caso de que las solicite y estén actualizadas. De esta manera, el cliente puede saltearse la comunicación directa con el servidor DNS.

Para obtener las direcciones IP correspondientes a un nombre de dominio, el Resolver hace consultas recursivas empezando desde el root server, hasta obtener la dirección completa.

# 6) Describa para qué se utilizan los siguientes tipos de registros de DNS:

| Registro | Descripción |
| --- | --- |
|**A** | Contiene el nombre de un dominio y la dirección IPv4 correspondiente a ese servidor |
| **MX** | Indica la redirección de los mensajes de correo a un servidor de correo. Permite establecer prioridades para distintos servidores de correo o redistribuir la carga entre ellos |
| **PTR** | Funciona al revés que el registro **A**. Contiene la dirección IPv4 correspondiente a un nombre de dominio. Sirve para realizar búsquedas inversas (buscar un dominio a partir de su IP) |
| **AAAA** | Similar al registro **A**, pero con direcciones IPv6. Contiene el nombre de un dominio y la dirección IPv6 correspondiente a ese servidor |
| **SRV** | Especifica servidores y puertos para acceder a servicios específicos. Sirven para acceder a un puerto específico de un servidor, puesto que los registros **A** o **AAAA** sólo indican direcciones |
| **NS** | Indica a qué servidor autoritativo corresponde un dominio específico. Sirven para saber en qué servidor buscar la dirección IP necesitada |
| **CNAME** | Redirecciona de un dominio a otro. A diferencia de **A**, en este registro los dominios no van a apuntar a una IP, sino que a otro dominio. Cuando un cliente recibe un registro de este tipo, debe hacer otra consulta al dominio al que se le redirige |
| **SOA** | Almacena información sobre el administrador de un dominio o zona de dominios |
| **TXT** | Almacena notas de texto escritas por el administrador del dominio |

# 7) En Internet, un dominio suele tener más de un servidor DNS. ¿Por qué cree que esto es así?
Son servidores con información replicada, para que se pueda responder de manera más rápida.

Tener servidores replicados permite que se distribuyan la carga de trabajo de las peticiones para no sobresaturarse, y también se pueden distribuir geográficamente. La desventaja es que se debe mantener la información de todos actualizada.

# 8) Cuando un dominio cuenta con más de un servidor, uno de ellos es el primario (o maestro) y todos los demás son los secundarios (o esclavos). ¿Cuál es la razón de que sea así?
El servidor maestro o primario es aquel que contiene la información autoritativa del dominio. Todas las configuraciones se realizan sobre éste.

Los servidores secundarios copian la información del maestro, por lo que siempre que se modifique el primario se les notifica para que vuelvan a copiar la información.

Esta división entre los servidores permite mantener la consistencia de los datos, aunque se traten de distintos almacenamientos. También permite que un dominio siga disponible en Internet aunque se haya caído su servidor primario, puesto que los secundarios van a seguir respondiendo.

# 9)  Explique brevemente en qué consiste el mecanismo de transferencia de zona y cuál es su finalidad.
La transferencia de zona es un tipo de transacción que se utiliza en DNS para replicar una base de datos en un conjunto de servidores. Se usa para copiar los datos de un servidor maestro en sus servidores secundarios.

Este tipo de transacciones se realizan para mantener la consistencia de los datos en un conjunto de servidores DNS, particularmente para que los secundarios tengan constancia de las modificaciones realizadas en el servidor primario.

# 10) Imagine que usted es el administrador del dominio de DNS de la UNLP (unlp.edu.ar). A su vez, cada facultad de la UNLP cuenta con un administrador que gestiona su propio dominio (por ejemplo, en el caso de la Facultad de Informática se trata de info.unlp.edu.ar). Suponga que se crea una nueva facultad, Facultad de Redes, cuyo dominio será redes.unlp.edu.ar, y el administrador le indica que quiere poder manejar su propio dominio. ¿Qué debe hacer usted para que el administrador de la Facultad de Redes pueda gestionar el dominio de forma independiente? (Pista: investigue en qué consiste la delegación de dominios).
Para que el administrador pueda manejar su propio dominio, hay que hacer una delegación. Esta consiste en cambiar el servidor al que apunta el dominio a uno nuevo, en este caso, al de la Facultad de Redes.

El nuevo servidor puede ser un servidor autoritativo, por lo que el administrador también va a poder gestionar los subdominios de `redes.unlp.edu.ar`.

# 11) Responda y justifique los siguientes ejercicios:

## a. En la VM, utilice el comando dig para obtener la dirección IP del host www.redes.unlp.edu.ar y responda:

<img src="./screenshots/Practica 3/ej11a.png">

### i. ¿La solicitud fue recursiva? ¿Y la respuesta? ¿Cómo lo sabe?
La respuesta fue recursiva. Esto lo sabemos porque tiene las siguientes flags:
* `ra`: Recursion allowed. Indica que el servidor acepta búsquedas recursivas para responder
* `rd`: Recursion desired. Indica que la solicitud fue recursiva o que debe iniciarse una recursión.

### ii. ¿Puede indicar si se trata de una respuesta autoritativa? ¿Qué significa que lo sea?
Es una respuesta autoritativa porque retorna el flag `aa` (Authoritative Answer). Esto significa que la respuesta fue recibida directamente desde el servidor autoritativo de ese dominio.

### iii. ¿Cuál es la dirección IP del resolver utilizado? ¿Cómo lo sabe?
La dirección el Resolver es 172.28.0.29. Lo sabemos porque el comando indica que se obtuvo la respuesta de ese servidor (en el renglón de `SERVER`).

## b. ¿Cuáles son los servidores de correo del dominio redes.unlp.edu.ar? ¿Por qué hay más de uno y qué significan los números que aparecen entre MX y el nombre? Si se quiere enviar un correo destinado a redes.unlp.edu.ar, ¿a qué servidor se le entregará? ¿En qué situación se le entregará al otro?

<img src="./screenshots/Practica 3/ej11b.png">

Usando la opción `MX` podemos ver los servidores de mail de la página.

Los servidores suelen tener varios servidores de email para balancear la carga de trabajo entre ellos o establecer prioridades para la entrega. El número entre MX y el servidor es la prioridad que tiene; cuanto menor sea el número, mayor es su prioridad.

Cuando se envía un mail, se le entrega al que tenga mayor prioridad. En este caso, se le intentará entregar a `mail.redes.unlp.edu.ar` (que tiene prioridad 5). En caso de que la entrega al primero falle, se le envía al segundo (en este caso, a `mail2.redes.unlp.edu.ar` con prioridad 10).

## c. ¿Cuáles son los servidores de DNS del dominio `redes.unlp.edu.ar`?

<img src="./screenshots/Practica 3/ej11c.png">

(NO SÉ SI ESTO ESTÁ BIEN)

Agregando la opción `NS` vemos que los servidores DNS son:
* `ns-sv-a.redes.unlp.edu.ar`
* `ns-sv-b.redes.unlp.edu.ar`

## d. Repita la consulta anterior cuatro veces más. ¿Qué observa? ¿Puede explicar a qué se debe?
Cambiaron:
* **La ID en el header de la respuesta.** El cliente crea un ID único para cada consulta DNS, por lo que cambia en cada una.
* **El valor de COOKIE en Pseudosection.** Las cookies están encriptadas para proteger los servidores DNS, por lo que es probable que el encriptado cambie en cada consulta.
* **El valor de WHEN en Authority Section.** Este valor indica la fecha y hora en la que se realizó la consulta, por lo que va a cambiar si se realizan en momentos distinso.

## e. Observe la información que obtuvo al consultar por los servidores de DNS del dominio. En base a la salida, ¿es posible indicar cuál de ellos es el primario?

<img src="./screenshots/Practica 3/ej11e-1.png">
<img src="./screenshots/Practica 3/ej11e-2.png">

Al consultar por cada servidor DNS, vemos que `ns-sv-b.redes.unlp.edu.ar` (172.28.0.29) tiene la misma dirección IP que la etiqueta SERVER de la sección de respuesta, mientras que `ns-sv-a.redes.unlp.edu.ar` tiene una distinta (172.28.0.30). Aunque no sé si con esto se puede suponer cuál es el primario

## f. Consulte por el registro SOA del dominio y responda:
<img src="./screenshots/Practica 3/ej11f.png">

### i. ¿Puede ahora determinar cuál es el servidor de DNS primario?
El registro SOA sigue el formato SERVIDOR PRIMARIO - SERVIDOR RAÍZ, por lo que el servidor primario es `ns-sv-b.redes.unlp.edu.ar`.

### ii. ¿Cuál es el número de serie, qué convención sigue y en qué casos es importante actualizarlo?
El número de serie es la "versión" actual del servidor primario, más particularmente de sus archivos y configuración. Este número cambia cada vez que el servidor primario es modificado.

Esto sirve para que los servidores secundarios sean notificados de los cambios y actualicen sus archivos propios mediante una transferencia de zona. En este ejemplo, el número de serie es 2020031700.

No estoy muy seguro del formato pero pareciera ser AÑO-MES-DÍA-NÚMEROS EXTRA, indicando la fecha de modificación (2020031700 -> 17 de Marzo del 2020).

### iii. ¿Qué valor tiene el segundo campo del registro? Investigue para qué se usa y como se interpreta el valor.
El segundo campo es el TTL (Time to Live) del registro. Esto indica cuánto tiempo tardarán en aplicarse los cambios realizados sobre dicho registro.

El tiempo está en segundos, por lo que si realizo un cambio sobre el registro SOA del ejemplo va a tardar 86400 segundos (24 horas) en aplicarse.

### iv. ¿Qué valor tiene el TTL de caché negativa y qué significa?
??????????????????????

## g.  Indique qué valor tiene el registro TXT para el nombre saludo.redes.unlp.edu.ar. Investigue para qué es usado este registro.
El registro TXT tiene el valor "HOLA".

Este registro se creó para que los administradores dejen notas legibles para los humanos, aunque también se le pueden agregar datos legibles por máquinas. Sin embargo, hoy en día es usado principalmente para la prevención de correos Spam y para verificar la propiedad de los dominios.

## h. Utilizando dig, solicite la transferencia de zona de redes.unlp.edu.ar, analice la salida y responda:
<img src="./screenshots/Practica 3/ej11h.png">

### i. ¿Qué significan los números que aparecen antes de la palabra IN? ¿Cuál es su finalidad?
Es el TTL del registro (contestado en la pregunta `11) F .iii` )

### ii. ¿Cuántos registros NS observa? Compare la respuesta con los servidores de DNS del dominio. ¿Puede explicar a qué se debe la diferencia y qué significa?
Son 4 registros. Los primeros 2 tienen como valor los servidores DNS del dominio (`ns-sv-a.redes.unlp.edu.ar` y `ns-sv-b.redes.unlp.edu.ar`), y los otros tienen como valor otros 2 servidores nuevos (`ns1.practica.redes.unlp.edu.ar` y `ns2.practica.redes.unlp.edu.ar`).

Los 2 nuevos no aparecían antes porque no son servidores DNS de `redes.unlp.edu.ar`, sino que pertenecen al subdominio de `practica.redes.unlp.edu.ar`.

# i. Consulte por el registro A de www.redes.unlp.edu.ar y luego por el registro A de www.practica.redes.unlp.edu.ar. Observe los TTL de ambos. Repita la operación y compare el valor de los TTL de cada uno respecto de la respuesta anterior. ¿Puede explicar qué está ocurriendo? (Pista: observar los flags será de ayuda)
<img src="./screenshots/Practica 3/ej11i-1.png">

<img src="./screenshots/Practica 3/ej11i-2.png">

El dig a `redes.unlp.edu.ar` tiene un TTL de 86400 estable. El dig a `practica.redes.unlp.edu.ar` tiene un TTL inicial de 8600, pero va bajando a medida que se realiza de nuevo la consulta.

Esto se debe a que la respuesta de `redes.unlp.edu.ar` viene del servidor autoritativo (lo sabemos por el flag `aa`), por lo que no requiere actualizar la información de sus registros. Por otro lado, la respuesta de `practica.redes.unlp.edu.ar` viene de un servidor no autoritativo, que depende del anterior. Debido a esto, puede tener información inconsistente y debe actualizarse, por lo que el TTL va disminuyendo y cuando llega a 0 solicita la información del servidor autoritativo de nuevo.

## j. Consulte por el registro A de www.practica2.redes.unlp.edu.ar. ¿Obtuvo alguna respuesta? Investigue sobre los codigos de respuesta de DNS. ¿Para qué son utilizados los mensajes NXDOMAIN y NOERROR?


<img src="./screenshots/Practica 3/ej11j.png">

Obtenemos una respuesta, pero con el código de estado `NXDOMAIN`. 

Los códigos de respuesta de DNS sirven para que el cliente conozca el estado de la consulta que realizó, y en el caso de que haya un error, sepa qué tipo de error fue.

* NOERROR: Indica que no hubo errores para realizar la consulta DNS y que se obtuvo una respuesta válida.
* NXDOMAIN: Indica que se realizó una petición a un dominio inexistente.

# 12) Investigue los comando nslookup y host. ¿Para qué sirven?
**NSLOOKUP** es un comando que sirve para hacer consultas DNS hacia un servidor; funciona de manera similar al `dig`, pero es más simple y muestra menos información.

**HOST** es un comando usado para obtener la dirección IP correspondiente a un nombre de dominio, o viceversa.

## a. Intente con ambos comandos obtener la dirección IP de `www.redes.unlp.edu.ar`

<img src="./screenshots/Practica 3/ej12a-1.png">

<img src="./screenshots/Practica 3/ej12a-2.png">

## b. Intente con ambos comandos obtener los servidores de correo del dominio `redes.unlp.edu.ar`

<img src="./screenshots/Practica 3/ej12b-1.png">

<img src="./screenshots/Practica 3/ej12b-2.png">

## c. Intente con ambos comandos obtener los servidores de DNS del dominio `redes.unlp.edu.ar`

<img src="./screenshots/Practica 3/ej12c-1.png">

<img src="./screenshots/Practica 3/ej12c-2.png">

# 13) ¿Qué función cumple en Linux/Unix el archivo `/etc/hosts` o en Windows el archivo `\WINDOWS\system32\drivers\etc\hosts`?
El archivo **HOSTS** es un archivo que usan los SO para traducir los nombres de dominios a direcciones IP. Esto sirve para que el sistema puede mapear nombres a direcciones IP localmente, sin necesidad de un servidor DNS.

Este es el archivo que usa nuestra computadora para reconocer que el nombre "localhost" apunta al puerto 127.0.0.1, por ejemplo.

# 14) Abra el programa Wireshark para comenzar a capturar el tráfico de red en la interfaz con IP 172.28.0.1. Una vez abierto realice una consulta DNS con el comando dig para averiguar el registro MX de redes.unlp.edu.ar y luego, otra para averiguar los registros NS correspondientes al dominio redes.unlp.edu.ar. Analice la información proporcionada por dig y compárelo con la captura.

## Servidores de email (registros MX):
<img src="./screenshots/Practica 3/ej14a-1.png">

<img src="./screenshots/Practica 3/ej14a-2.png">

## Servidores de DNS (registros NS):
<img src="./screenshots/Practica 3/ej14b-1.png">

<img src="./screenshots/Practica 3/ej14b-2.png">

# 15) Dada la siguiente situación: “Una PC en una red determinada, con acceso a Internet, utiliza los servicios de DNS de un servidor de la red”. Analice:

## a. ¿Qué tipo de consultas (iterativas o recursivas) realiza la PC a su servidor de DNS?
La PC realiza consultas recursivas desde su resolver hacia el servidor DNS.

## b. ¿Qué tipo de consultas (iterativas o recursivas) realiza el servidor de DNS para resolver requerimientos de usuario como el anterior? ¿A quién le realiza estas consultas?
El servidor DNS que intenta resolver la consulta recursiva, realizará una serie de consultas iterativas a los distintos servidores DNS que sean necesarios hasta llegar al servidor DNS autoritativo para el dominio solicitado.

# 16) Relacione DNS con HTTP. ¿Se puede navegar si no hay servicio de DNS?
HTTP es un protocolo de transferencia de archivos, mientras que DNS es un protocolo de mapeo de direcciones IP a nombres de dominio.

Lo que hace DNS es ofrecer un sistema de servidores completo para acceder a las direcciones de los servidores HTTP a partir de un formato legible por los usuarios, para que estos puedan acceder más fácilmente.

HTTP podría funcionar sin DNS, puesto que permite referenciar servidores mediante su dirección IP, pero tendría los siguientes problemas:
* Las computadoras tendrían que tener el mapeo de dirección IP - dominio localmente (en el archivo `hosts`), o el usuario tendría que acordarse de memoria las IPs.
* Las direcciones IP de los servidores HTTP no son estáticas. Si no hubiese DNS, habría que cambiar el mapeado o consultar a una IP nueva cada vez que cambie.

# 17) Observar el siguiente gráfico y contestar:
<img src="./screenshots/Practica 3/ej17.png">

## a. Si la PC-A, que usa como servidor de DNS a "DNS Server", desea obtener la IP de `www.unlp.edu.ar`, cuáles serían, y en qué orden, los pasos que se ejecutarán para obtener la respuesta.
1. PC-A realiza una consulta recursiva al DNS Server para obtener la dirección `www.unlp.edu.ar`.
2. DNS Server realiza una consulta iterativa `Root-Server` y recibe la dirección IP del servidor que controla el TLD del dominio (en este caso, el TLD es `.ar`, controlado por `a.dns.ar`)
3. El DNS Server realiza una consulta iterativa a `a.dns.ar` y recibe la dirección IP del servidor `ns1.riu.edu.ar`, que es autoritativo para el dominio `edu.ar`.
4. El DNS Server realiza una consulta iterativa a `ns1.riu.edu.ar` y recibe la dirección IP del servidor `unlp.unlp.edu.ar`, que es autoritativo para el dominio `unlp.edu.ar`.
5. El DNS realiza una consulta iterativa a `unlp.unlp.edu.ar` y recibe finalmente la dirección IP de `www.unlp.edu.ar`, puesto que todos sus registros se encuentran en este servidor.

# 18) ¿A quién debería consultar para que la respuesta sobre www.google.com sea autoritativa?

<img src="./screenshots/Practica 3/ej18-1.png">

Revisando los registros SOA vemos que el servidor raíz de `www.google.com` es `dns1.google.com`, por lo que si consultamos por el dominio de Google en ese servidor vamos a obtener la respuesta autoritativa.

<img src="./screenshots/Practica 3/ej18-2.png">

Sabemos que la respuesta es autoritativa porque tiene el flag de `aa` (Authoritative Answer).

# 19) ¿Qué sucede si al servidor elegido en el paso anterior se lo consulta por www.info.unlp.edu.ar? ¿Y si la consulta es al servidor 8.8.8.8?

<img src="./screenshots/Practica 3/ej19-1.png">

Si le consultamos al servidor `dns1.google.com` la respuesta tiene el código de estado **REFUSED**. Esto se debe a que ese servidor no tiene registros asociados con ninguno de los dominios de `www.info.unlp.edu.ar`, por lo que no puede darnos una respuesta.

<img src="./screenshots/Practica 3/ej19-2.png">

El servidor `8.8.8.8` es la dirección IP de Google Public DNS, un servicio para almacenar nombres de dominio gratuito. Como este servicio tiene respuestas para los TLD de `www.info.unlp.edu.ar`, puede realizarse la consulta DNS sin problemas.