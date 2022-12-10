---
description: >-
  Correo electrónico con Postfix + Dovecot + mailutils Thunderbird en Ubuntu
  22.04
---

# Postfix

<mark style="color:red;">Propuesta:</mark>&#x20;

1. <mark style="color:red;">Hacer la instalación en la propia MV de Ubuntu Server 22.04 donde tengo configurado el dominio</mark> <mark style="color:red;"></mark>_<mark style="color:red;"><mark style="color:blue;">`haven.local`<mark style="color:blue;"></mark>_ <mark style="color:red;"></mark><mark style="color:red;">y los servicios de DNS y DCHP.</mark>
2. <mark style="color:red;">Instalar el servidor de correo en una MV con Ubuntu Server 22.04, bajo el dominio de haven.local.</mark>

El objetivo es mostrar el funcionamiento de un servidor de  correo electrónico. Para ello usaremos una MV con Ubuntu Desktop 22.04 LTS. Vamos a necesitar instalar los siguientes servicios:

* ``[`Dovecot-imapd`](https://elpuig.xeill.net/Members/vcarceler/articulos/introduccion-a-dovecot) `` como servidor IMAP
* ``[`Postfix`](https://elpuig.xeill.net/Members/vcarceler/articulos/introduccion-al-correo-electronico-con-postfix) como servidor de correo SMTP - (MTA)&#x20;
* ``[`Mailutils`](https://mailutils.org/) `` como cliente para el terminal (MUA)
* ``[`Thunderbird`](https://www.thunderbird.net/) **** como cliente, en un entorno gráfico

Como se trata de un ejercicio académico, intentaremos simplificar la instalación para lo cual se utilizará la configuración más sencilla posible.&#x20;

### El entorno de la MV

La idea es utilizar dos MV para realizar las pruebas.

1. **Adaptador1**: En modo puente  con la interfaz de red del equipo anfitrión.
2. Vamos a fijar el dominio del correo: <mark style="color:blue;">`arrakis.local`</mark> en el archivo <mark style="color:blue;">`/etc/hosts`</mark>. Para ello hacemos lo siguiente:

![](<../../.gitbook/assets/image (122).png>)

### Instalación

Tenemos que preparar el sistema antes de hacer las instalaciones de: &#x20;

* el  servidor SMTP: Postfix
* el servidor IMAP: Dovecot
* y para gestionar los correos: MailUtils y Thunderbird&#x20;

Lo primero será actualizar el sistema:

<mark style="color:blue;">`apt update`</mark>

Una vez actualizado el sistema, procedemos con la instalación. Yo prefiero instalar paquete a paquete.&#x20;

### Postfix _<mark style="color:blue;">``</mark>_&#x20;

<mark style="color:blue;">`sudo apt-get install postfix`</mark>

![](<../../.gitbook/assets/image (84).png>)

Utilizaremos como nombre de dominio: **** _<mark style="color:blue;">`arrakis.local`</mark>_

![](<../../.gitbook/assets/image (127).png>)

#### Configuración de Postfix

En <mark style="color:blue;">`/etc/postfix`</mark> están los archivos de configuración del servicio:&#x20;

![](<../../.gitbook/assets/image (156).png>)

Existen dos formatos muy extendidos para almacenar el correo electrónico. Estos formatos son [_Mbox_](https://en.wikipedia.org/wiki/Mbox) y [Maildir](https://en.wikipedia.org/wiki/Maildir).

_<mark style="color:blue;">Mbox</mark>_ permite guardar todos los mensajes en un solo archivo, y _<mark style="color:blue;">Maildir</mark> _ utiliza un directorio para guardar los mensajes en ficheros individuales.

En la configuración por defecto de Postfix y mailutils se utiliza el formato _<mark style="color:blue;">Mbox</mark>_, pero ambos soportan _<mark style="color:blue;">Maildir</mark>_, así que vamos a utilizar este formato. Para ello vamos a modificar el archivo _<mark style="color:blue;">main.cf</mark>_ de Postfix.&#x20;

Una práctica conveniente es hacer una copia de seguridad  de los archivos de configuración que vayamos a modificar. Para ello, hacemos:

<mark style="color:blue;">`sudo cp /etc/postfix/main.cf /etc/postfix/main.cf.BKP`</mark>

Editamos el archivo _<mark style="color:blue;">main.cf</mark>_ y vamos a agregar las siguientes líneas al final del mismo.&#x20;

![](<../../.gitbook/assets/image (72).png>)



### Dovecot

Antes de hacer la instalación expliquemos un mínimo en qué consiste.

[Dovecot](https://www.dovecot.org/) es un [MDA](https://en.wikipedia.org/wiki/Mail\_delivery\_agent) que tiene como función almacenar los correos y servirlos mediante los protocolos [POP3](https://en.wikipedia.org/wiki/Post\_Office\_Protocol) o [IMAP4](https://en.wikipedia.org/wiki/Internet\_Message\_Access\_Protocol) al programa cliente [MUA](https://es.wikipedia.org/wiki/Cliente\_de\_correo\_electr%C3%B3nico).

1. **POP3.** Protocolo sencillo pensado para descargar el correo desde el servidor y después borrarlo, aunque es posible mantenerlo en el servidor.  No está pensado para funcionar en escenarios en los que varios dispositivos comprueben el correo del mismo buzón.
2. **IMAP4.** Protocolo más complejo, puesto que se tiene en cuenta la posibilidad que varios clientes puedan acceder al mismo buzón de manera simultánea. Esto hace que el protocolo deba permitir realizar operaciones para mantener ordenado el buzón de correo desde el cliente, descargando tan solo, aquellos ficheros a los que el usuario tiene acceso.
3. Lógicamente, ambos protocolos cuentan con su versión TLS.

#### Instalación de Dovecot

Para instalar, hacemos como siempre:&#x20;

_<mark style="color:blue;">`sudo apt-get install dovecot-imapd`</mark>_&#x20;

Una vez instalado hagamos las comprobaciones pertinentes. Podemos observar que Dovecot:

* Está escuchando en los puertos 143 y 993 que corresponden a IMAP e IMAPS.
* Utiliza el mismo log file <mark style="color:blue;">`/var/log/mail.log`</mark> que el MTA.

La configuración está dividida en varios ficheros dentro del directorio <mark style="color:blue;">`/etc/dovecot`</mark>&#x20;

![](<../../.gitbook/assets/image (77).png>)

El archivo principal se encuentra en:  <mark style="color:blue;">`/etc/dovecot/dovecot.conf`</mark> e incluye a los otros ficheros.&#x20;

Para ver los cambios que se han hecho en alguno de los ficheros, ejecuta el siguiente comando y te mostrará un fichero de configuración con los cambios:&#x20;

<mark style="color:blue;">`dovecot -n`</mark> &#x20;

![](<../../.gitbook/assets/image (180).png>)



Dovecot puede trabajar tanto con mbox como con mailbox y como es de suponer, la configuración por defecto es con mbox, pero podemos hacer el cambio. Para ello buscamos la directiva  <mark style="color:blue;">`mail_location`</mark> en el archivo: <mark style="color:blue;">`/etc/dovecot/conf.d/10-mail.conf`</mark>**.**

Hacemos la copia de seguridad del archivo de configuración:

<mark style="color:blue;">`sudo cp /etc/dovecot/conf.d/10-mail.conf /etc/dovecot/conf.d/10-mail.conf.BKP`</mark>

Observa en este punto del archivo de configuración <mark style="color:blue;">`10-mail.conf`</mark> que nos dice explícitamente que podemos hacer el cambio que os digo. Así que vamos allá.

![](<../../.gitbook/assets/image (153).png>)

Editamos el archivo:

<mark style="color:blue;">`sudo nano /etc/dovecot/conf.d/10-mail.conf`</mark> y buscamos esta directiva <mark style="color:blue;">`mail_location`</mark> y la sustituimos por lo que nos dice. Esto es:&#x20;

<mark style="color:blue;">`mail_location = maildir:~/Maildir`</mark>

![](<../../.gitbook/assets/image (63) (1).png>)

### Cliente de correo: Mailutils

Procederemos a la instalación de<mark style="color:purple;">l</mark> cliente de correo _<mark style="color:blue;">`mailutils`</mark>_. Para ello ejecutamos:

_<mark style="color:blue;">`sudo apt-get install mailutils`</mark>_

![](<../../.gitbook/assets/image (18) (1).png>)

**Nota**: Puedes utilizar otro cliente de correo por terminal como por ejemplo: [Mutt](../clientes-de-correo/mutt.md)

#### Testeando&#x20;

Para probar el servicio de correo vía terminal basta escribir algo como lo siguiente:

<mark style="color:blue;">`echo "Aquí va el body del email" | mail -s "Aquí va el asunto" your_email_address`</mark>` ```&#x20;

Si vamos al home del único usuario instalado en el sistema, "kirby", veremos que existe un directorio llamado <mark style="color:blue;">`Maildir`</mark> que contiene la estructura básica de un buzón de correos. Vamos a la carpeta o directorio <mark style="color:blue;">`/home/kirby/Maildir/new`</mark> y vemos que hay dos correos sin enviar.

![](<../../.gitbook/assets/image (31).png>)

Observemos el contenido del primero:

![](<../../.gitbook/assets/image (71).png>)

y nos percatamos que no ha sido posible la entrega del correo enviado. Es evidente que todavía nos queda mucho por configurar.

![](<../../.gitbook/assets/image (186).png>)

### Cliente de correo: Thunderbird

La guía para instalar Thunderbird la puedes encontrar en este [enlace](broken-reference). Sin embargo, es muy sencillo desde el propio terminal:

<mark style="color:blue;">`sudo apt-get install thunderbird`</mark>

Lo más probable es que no necesites hacer la instalación porque viene por defecto en Ubuntu Desktop.

#### Testeando&#x20;

Probemos el servidor IMAP  con Thunderbird como MUA. Lo primero que tendremos que hacer es configurar la cuenta de correo de uno de los <mark style="color:blue;">usuarios locales</mark>.&#x20;

Quiero hacer énfasis en esto de usuarios locales, porque se trata de los propios usuarios del sistema, del servidor de correos. Hay un modo de configurar un servidor de correos con Postfix sin necesidad de tener los usuarios en el sistema, sino que estarían en una DB como MySQL.  Intentaré hacer la adaptación para ese caso porque resulta muy interesante.

Entonces, voy a crear otro usuario en el sistema, además del usuario <mark style="color:blue;">`kirby`</mark>. Para ello, puedes hacer:

<mark style="color:blue;">`sudo adduser pepe`</mark>&#x20;

y ya lo tenemos:

&#x20;

![](<../../.gitbook/assets/image (168).png>)

Configuramos la cuenta de Pepa en Thunderbird. La contraseña es la misma del sistema, claro está. Un detalle adicional, es evidente que es necesario aceptar una excepción para el certificado digital porque tampoco está configurado.

![](<../../.gitbook/assets/image (187).png>)

Hace las verificaciones y encuentra que:

![](<../../.gitbook/assets/image (158).png>)

Entre otros datos nos dice que se pueden cifrar los datos de extremo a extremo y agregar una firma. Se puede utilizar [`GPG`](https://gnupg.org/) para crear una par de claves [`pública - privada`](https://www.cloudflare.com/es-es/learning/ssl/how-does-public-key-encryption-work/) que permita cifrar los mensajes.

![](<../../.gitbook/assets/image (7) (1) (1).png>)

Desde el usuario pepe@arrakis.local enviamos un correo a pepa@arrakis.local (recuerda que ambos están en el sistema):&#x20;

![](<../../.gitbook/assets/image (128).png>)

Y enseguida podemos observar que llega un correo nuevo al buzón de Pepa:

![](<../../.gitbook/assets/image (45).png>)

Sin lugar a dudas, faltan muchos detalles por configurar. Entre los tantos detalles:&#x20;

1. no hay ningún filtro antispam, como por ejemplo  [Spamassassin](https://spamassassin.apache.org).&#x20;
2. no hemos probado el servicio desde otro equipo que no sea el propio servidor
3. no tenemos un servidor de DNS
4. etc.

Nos queda mucho por trabajar pero hemos aprendido algo.

### Verificando el servicio con telnet

Para poder comprobar que efectivamente esto es así, en el servidor de correos he modificado el adaptador de red al <mark style="color:blue;">`modo puente`</mark> para estar en la misma red que el equipo anfitrión.&#x20;

Hacemos telnet a la IP o nombre del servidor de correos a través del puerto 25 de SMTP:

<mark style="color:blue;">`telnet 192.168.1.30 25`</mark>

Una vez dentro nos responde lo siguiente:

<mark style="color:blue;">`220 kirby.home ESMTP Postfix (Ubuntu)`</mark>

Si observamos, no está respondiendo con el dominio arrakis.local que le asignamos (<mark style="color:red;">detalle a corregir</mark>)

Por otra parte, una forma de comunicarnos con el servidor es escribiendo:

<mark style="color:blue;">`ehlo kirby`</mark> o <mark style="color:blue;">`ehlo localhost`</mark>

Y nos responde con:

![](<../../.gitbook/assets/image (44).png>)

Como se muestra en la figura, podemos escribir un correo electrónico utilizando:

<mark style="color:blue;">`mail from: usuario@dominio`</mark>

Si nos responde con un <mark style="color:blue;">`250 2.1.0 OK`</mark> entonces podemos escribir el destinatario. Observa que he escrito <mark style="color:blue;">`kirby@kirby`</mark> y esto es porque el usuario que tengo en el sistema es <mark style="color:blue;">`kirby`</mark> y el nombre del equipo también (<mark style="color:red;">por el error que ya he comentado</mark>).

<mark style="color:blue;">`rcpt to: kirby@kirby`</mark>

Y como está correcto, nos responde con un <mark style="color:blue;">`250 2.1.5 OK`</mark>

En este punto podemos escribir data y en la siguiente línea escribir el cuerpo del mensaje. Para finalizar, clicamos <mark style="color:blue;">`enter`</mark> y ponemos un punto. Observa que nos responde el servidor con un <mark style="color:blue;">`250 2.0.0 OK`</mark> con el mensaje enviado.  El comando <mark style="color:blue;">`quit`</mark> para salir de la conexión.

¿Y ahora qué? Pues vayamos a nuestro servidor de correos y miremos en la carpeta <mark style="color:blue;">`/home/kirby/Mailbox/new/1658955914.....kirby`</mark> (el último recibido a las 23:05) y es nuestro correo que ha llegado al servidor.

![](<../../.gitbook/assets/image (164).png>)

Está claro que tenemos ciertos puertos abiertos para la recepción y envío de los correos en el servidor:

<mark style="color:blue;">`sudo nmap -ST -= 192.168.1.30`</mark>

![](<../../.gitbook/assets/image (2) (1) (1) (1).png>)

### Link

1. [https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-as-a-send-only-smtp-server-on-ubuntu-20-04-es](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-as-a-send-only-smtp-server-on-ubuntu-20-04-es)&#x20;
2. [https://mailutils.org/manual/mailutils.pdf](https://mailutils.org/manual/mailutils.pdf)&#x20;
3. <mark style="color:purple;"></mark>[<mark style="color:purple;">https://www.dovecot.org/</mark>](https://www.dovecot.org/) <mark style="color:purple;"></mark>&#x20;
