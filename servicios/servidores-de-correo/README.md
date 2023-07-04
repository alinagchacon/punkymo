# Servidores de correo

### ¿Qué son los servidores de correo?

El correo electrónico es uno de los servicios más utilizados en Internet, y está basado en una serie de protocolos que permiten la descarga, lectura y envío de correos.&#x20;

Estamos tan acostumbrados a utilizar el correo electrónico que simplificamos el proceso de comunicación que ocurre detrás de cada envío y recibo de emails. A nivel empresarial su uso es imprescindible y  es una herramienta que en muchas ocasiones tiene importancia estratégica. Por tanto, estamos hablando de un servicio indispensable a día de hoy tanto en el ámbito laboral como personal. Pudiéramos decir que un servidor de correo es una aplicación que:

1. Recibe correos de otros usuarios
2. Distribuye correos a otros servidores de correo
3. Gestionan y entregan correos a los destinatarios correspondientes

Independientemente que exista una cierta variedad de servidores de correo, gratis, de pago, para Windows o no, los servidores de correo comparten ciertas funcionalidades entre sí, como por ejemplo:

* El servicio de Webmail. Acceso al correo vía navegador.
* El acceso móvil
* Antispam y seguridad
* Antivirus

Algunos **servidores de correo electrónicos** muy conocidos son:&#x20;

* Gmail de Google
* Hotmail
* Yahoo
* Microsoft Office 365
* Zoho

Por cierto, un detalle: El día 9 de Octubre se celebra el Día Mundial del Correo. Se ha seleccionado este día porque coincide con el aniversario del establecimiento de la <mark style="color:blue;">`Unión Postal Universal`</mark> en **1874** en Berna,  Suiza. En  1969 fue declarado Día Mundial del Correo por el Congreso de la UPU (Unión Postal Universal) celebrado en Tokio, Japón.

#### Protocolos POP3, IMAP, SMTP

Los protocolos más importantes que intervienen en el proceso de envío y recepción de los correos electrónicos son: POP3, IMAP y SMTP.&#x20;

POP3 es uno de los protocolos que más se ha utilizado, aunque en los últimos años IMAP se ha popularizado y ha relegado al POP3. Hagamos un repaso de las principales características de cada uno de ellos.

<table><thead><tr><th width="146">Protocolo</th><th width="214">Descripción</th><th width="310">Usos</th></tr></thead><tbody><tr><td>SMTP</td><td><a href="https://es.wikipedia.org/wiki/Protocolo_para_transferencia_simple_de_correo">Simple Mail Transfer Protocol</a> o Protocolo simple de transferencia de correo</td><td>Envía correo a otros destinatarios. Tanto los usuarios que emplean POP como aquellos que optan por IMAP pueden utilizarlo para enviar sus mensajes configurándolo en su cliente de correo.</td></tr><tr><td>ESMTP</td><td>SMTP extendido</td><td><p>Con el incremento de los usuarios de correo electrónico, las funcionalidades de los correos se han ampliado, y se hizo necesaria la extensión del protocolo SMTP para incluir nuevas capacidades al protocolo clásico que era muy básico.</p><p></p><p>La mayor parte de servidores de correo utilizados en la actualidad proporcionan a sus usuarios capacidades SMTP extendidas, siendo siempre compatibles con el protocolo SMTP original.</p></td></tr><tr><td>IMAP</td><td><a href="https://es.wikipedia.org/wiki/Protocolo_de_acceso_a_mensajes_de_Internet">Internet Message Access Protocol</a> o Protocolo de acceso a los mensajes de Internet.</td><td>Permite al administrador crear filtros y carpetas desde el propio servidor. Los mensajes permanecen en el mismo y el correo puede gestionarse indistintamente a través de correo web o mediante un cliente como Outlook.</td></tr><tr><td>POP o POP3</td><td><a href="https://es.wikipedia.org/wiki/Protocolo_de_oficina_de_correo">Post Office Protocol</a> o Protocolo de oficina de correo</td><td>Requiere de un cliente de correo para la gestión del buzón. Por lo general, tras la descarga del e-mail éste se elimina del servidor.</td></tr></tbody></table>

### Funcionamiento de los servidores de correo

#### Desde un punto de vista técnico

Al enviarse un correo electrónico, la información se transmite de servidor a servidor hasta aquel que contiene el correo electrónico del destinatario. Esto es, la información se envía al servidor responsable de transferir el correo y es el que se denomina  <mark style="color:blue;">`Agente de Transporte de Correo (MTA).`</mark>

El <mark style="color:blue;">`MTA`</mark> del destinatario entrega el correo electrónico al servidor de recepción de correo o <mark style="color:blue;">`Agente de entrega de correo (MDA)`</mark> y éste  lo guardará hasta que el usuario lo reciba, a través de uno de los dos protocolos para recibir los correos de un <mark style="color:blue;">`MDA`</mark>: <mark style="color:blue;">`POP3`</mark> o <mark style="color:blue;">`IMAP`</mark>.

Dicho de un modo simple: el <mark style="color:blue;">`MTA`</mark> es la oficina de correos y el <mark style="color:blue;">`MDA`</mark> el buzón, que almacena el correo a la espera que el destinatario revise el buzón.

Por otra parte, el programa que recibe el correo es llamado <mark style="color:blue;">`Mail User Agent (MUA)`</mark>, esto es: un cliente de correo como **Thunderbird** o **Outlook**.

**Resumiendo**:

1. **MDA** o [Mail Delivery Agent](https://en.wikipedia.org/wiki/Mail\_delivery\_agent) o Agente de entrega de correo (el buzón de correos).  Es quien mantiene los buzones de los usuarios, para que sus MUA o clientes de correo se conecten y descarguen el correo recibido. Ejemplos de MDA muy utilizados son [Dovecot](https://www.dovecot.org/), [maildrop ](https://en.wikipedia.org/wiki/Maildrop)y [Courier](http://www.courier-mta.org).
2.  **MTA** o [Mail Transfer Agent](https://es.wikipedia.org/wiki/Servidor\_de\_correo) o Agente de Transporte de Correo (la oficina de correos). Es el servidor de correo que recoge los correos que envían sus usuarios y los entrega a los MTA de los destinatarios. Ejemplos de algunos MTA son: [Postfix](https://www.postfix.org), [Exim](https://es.wikipedia.org/wiki/Exim), [Sendmail](https://es.wikipedia.org/wiki/Sendmail) y [Qmail](https://es.wikipedia.org/wiki/Qmail).


3. **MUA** o [Mail User Agent](https://es.wikipedia.org/wiki/Cliente\_de\_correo\_electr%C3%B3nico). El cliente de correo. Es el programa cliente que utiliza el usuario final para enviar y recibir correos. Puede ser desde una sencilla herramienta por línea de comandos como [<mark style="color:blue;">`Mail`</mark> ](https://mailutils.org/manual/html\_section/mail.html)y brindada por [<mark style="color:blue;">`mailutils`</mark>](https://mailutils.org/manual/html\_section/mail.html), hasta una herramienta gráfica como[ Thunderbird](https://www.thunderbird.net/) o una herramienta web como [Roundcube](https://roundcube.net/).



<figure><img src="../../.gitbook/assets/image (19) (1) (1).png" alt=""><figcaption><p>Imagen tomada de la Wikipedia</p></figcaption></figure>

#### Desde un punto de vista más simple

Supongamos que Pepe (<mark style="color:blue;">pepe@dominioA.com</mark>) quiere enviar un correo electrónico a María (<mark style="color:blue;">maria@dominioB.com</mark>).

La historia comienza cuando <mark style="color:blue;">pepe@dominioA.com</mark> desde un programa cliente en su PC, web o desde una aplicación móvil, escribe un correo con destino a <mark style="color:blue;">maria@dominioB.com</mark>.

El programa cliente no envía directamente el correo al destinatario (María en este caso). Lo único que hace es transmitir (vía protocolo SMTP) el correo al servidor de correo (MTA) encargado del correo saliente en el dominioA que utiliza Pepe. Digamos que el servidor de correo (para Pepe) es el <mark style="color:blue;">smtp.dominioA.com</mark>.

A día de hoy, los servidores de correo (MTA) solo aceptan correos de los MUA de sus propios usuarios. A diferencia de otros tiempos en los que cualquier servidor de correo de Internet aceptaba correos de los programas clientes (aunque no fueran sus usuarios) y enviaba esos correos a los servidores destinatarios.

Bueno, ¿Y qué hace el servidor de correo smtp.dominioA.com? Pues examina el correo y descubre que el destinatario es <mark style="color:blue;">maria@dominioB.com</mark>. Siguiente cuestión: ¿Quién se encarga del correo para el dominio <mark style="color:blue;">dominioB.com</mark>?

Pues aquí entra en funcionamiento el [`DNS`](https://es.wikipedia.org/wiki/Sistema\_de\_nombres\_de\_dominio): se hace una consulta al servidor DNS que permite encontrar los servidores que recogen el correo para los dominios en Internet. Para este fin se utilizan los registros <mark style="color:blue;">`MX`</mark> que permiten indicar qué servidor o servidores se encargan de esta función.

Podemos utilizar la herramienta host para obtener los registros <mark style="color:blue;">`MX`</mark> de <mark style="color:blue;">`dominioB.com`</mark> del siguiente modo:

<mark style="color:blue;">`host -t MX dominioB.com`</mark>

Como dominioB.com no es real, la figura siguiente muestra el comando para dominios de Internet:

![](<../../.gitbook/assets/image (150).png>)

Supongamos que existe un único servidor <mark style="color:blue;">`smtp.dominioB.com`</mark>, con lo cual intenta hacer una conexión SMTP para entregar el correo de Pepe. Sin embargo, es muy habitual listar varios servidores <mark style="color:blue;">`MX`</mark> para un mismo dominio. Por ejemplo, haz la consulta para la <mark style="color:blue;">wikipedia.org</mark>, <mark style="color:blue;">planeta.es</mark>, <mark style="color:blue;">dell.com</mark>.&#x20;

De la lista de los servidores posibles para un dominio existe uno que está anunciado con cierta prioridad sobre los otros, y donde resulta que los números bajos tienen más prioridad. Esto nos indica que siempre se intentará entregar el correo al servidor más prioritario y, en caso de no estar disponible, se intentará entregar el correo al siguiente prioritario en la lista.

Una vez que el correo de Pepe ha sido entregado al servidor (de María) el correo permanece en el mismo hasta que el MUA (cliente de correo) se conecte para consultar el correo. Este correo puede estar en formato <mark style="color:blue;">`Mbox`</mark> o <mark style="color:blue;">`Maildir`</mark>.

Entonces, nos queda claro que el funcionamiento de un MTA depende del servicio DNS, con lo cual es recomendable instalar Postfix después de tener  debidamente configurados los registros <mark style="color:blue;">`MX`</mark> en nuestro servidor <mark style="color:blue;">`DNS`</mark>.

Veamos a continuación algunos tipos de servidores de IMAP y POP3.

## Dovecot - un MDA

Se trata de un servidor de IMAP y POP3 de código abierto para Linux. Fue publicado en julio del 2002 y tiene entre sus características ser ligero, rápido, fácil de instalar y  seguro.

Actúa como servidor de almacenamiento de correo. El correo es entregado al servidor mediante algún agente de entrega de correo o MDA y se almacena para su uso a través de un cliente de correo  o MUA.&#x20;

También puede actuar como servidor proxy de correo, reenviar la conexión a otro servidor de correo o actuar como un MUA ligero para recuperar y manipular el correo en un servidor remoto, por ejemplo, para la migración de correo.

Admite variedad de esquemas de autenticación para acceder vía IMAP, POP3 y SMTP, incluido CRAM-MD5 y DIGEST-MD5.

## Postfix - un MTA

Se trata de un servidor de correos creado por [Wietse Venema](http://www.porcupine.org/wietse/) como alternativa al programa ampliamente utilizado [Sendmail](https://www.proofpoint.com/us/products/email-protection/open-source-email-solution).&#x20;

Algunas características de Postfix son:

1. Rápido, fácil de administrar y seguro.&#x20;
2. Puede ejecutarse en sistemas similares a UNIX, incluidos AIX, BSD, HP-UX, Linux, MacOS X, Solaris y más.
3. Se distribuye como código listo para ejecutar por proveedores de sistemas operativos, proveedores de dispositivos y otros proveedores.&#x20;

Si quieres acceder al sitio oficial clica en este [enlace](https://www.postfix.org).&#x20;

### Verificar el servicio&#x20;

Podemos verificar de forma rápida y sencilla si un servidor de correo SMTP está funcionando de forma correcta o no. Lo más sencillo es hacerlo es a través del protocolo de red [TELNET](https://es.wikipedia.org/wiki/Telnet).

Desde el intérprete de comandos o <mark style="color:blue;">`CMD`</mark> podemos conectarnos vía <mark style="color:blue;">TELNET</mark> a nuestro servidor de correos a través del puerto 25 utilizado por el protocolo [SMTP](https://es.wikipedia.org/wiki/Protocolo\_para\_transferencia\_simple\_de\_correo).&#x20;

Para poder comprobar que efectivamente esto es así, he modificado el adaptador de red al modo puente para estar en la misma red que el equipo anfitrión.&#x20;

Hacemos:

<mark style="color:blue;">`telnet X.X.X.X 25`</mark>

Una vez dentro nos responde lo siguiente:

<mark style="color:blue;">`220 X ESMTP Postfix (Ubuntu)`</mark>

Si observamos, no está respondiendo con el dominio <mark style="color:blue;">`arrakis.local`</mark> que le asignamos (<mark style="color:red;">detalle a corregir</mark>)

Por otra parte, una forma de comunicarnos con el servidor es escribiendo:

<mark style="color:blue;">`ehlo X.X.X.X`</mark> o <mark style="color:blue;">`ehlo localhost`</mark>

Y nos responde con:

<mark style="color:blue;">`mail from: usuario@dominio`</mark>

Si nos responde con un <mark style="color:blue;">`250 2.1.0 OK`</mark> entonces podemos escribir el destinatario.&#x20;

<mark style="color:blue;">`rcpt to: usuario@dominio`</mark>

Y como está correcto, nos responde con un <mark style="color:blue;">`250 2.1.5 OK`</mark>

En este punto podemos escribir data y en la siguiente línea escribir el cuerpo del mensaje. Para finalizar, clicamos <mark style="color:blue;">`enter`</mark> y ponemos un punto. Observa que nos responde el servidor con un <mark style="color:blue;">`250 2.0.0 OK`</mark> con el mensaje enviado.  El comando <mark style="color:blue;">`quit`</mark> para salir de la conexión.



### Links

* [https://www.incibe.es/protege-tu-empresa/te-ayudamos/fraude-email-comprometido](https://www.incibe.es/protege-tu-empresa/te-ayudamos/fraude-email-comprometido)&#x20;
* [https://www.incibe.es/sites/default/files/contenidos/politicas/documentos/uso-correo-electronico.pdf](https://www.incibe.es/sites/default/files/contenidos/politicas/documentos/uso-correo-electronico.pdf)
* [https://www.un.org/es/observances/world-post-day](https://www.un.org/es/observances/world-post-day)

##
