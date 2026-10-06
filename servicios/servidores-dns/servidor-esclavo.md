# Servidor esclavo

Un _**servidor DNS secundario o esclavo**_ es un servidor que obtiene los RR del fichero de zona a través de operaciones de transferencia de zonas. Las respuestas de este servidor secundario son tan autoritarias como las de un servidor maestro, siempre que sus datos no hayan expirado, o sea, el tiempo de expiración del RR SOA. También, es interesante recordar que los servidores esclavos pueden hacer transferencias de zonas a otros esclavos.

La configuración que vamos a llevar a cabo, hará que dispongamos de un servidor esclavo con las siguientes características:

1\) Un servidor esclavo en el dominio _haven.local_, incluyendo su zona inversa. Esto se consigue creando una zona de tipo _<mark style="color:violet;">slave</mark>_.

2\) Un servidor que proporciona el servicio de consultas recursivas, esto es, el servidor acepta consultas y se encargará de conseguir las respuestas, que finalmente serán entregadas al solicitante. Esto se consigue con la opción _<mark style="color:violet;">recursion yes;</mark>_.

3\) Un servidor caché para el resto de dominios que sean distintos a _<mark style="color:violet;">haven.local</mark>_. Para ello, utilizamos la opción: _<mark style="color:violet;">recursion yes;</mark>_ y a la zona "." (zona raíz).

La configuración del fichero _named.conf_ la vamos a distribuir de la misma forma que en la configuración del servidor maestro.
