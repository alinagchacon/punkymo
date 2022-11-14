# Servidores Web

Los servidores web se utilizan para la entrega de contenido web, permitiendo transferir documentos a los clientes como páginas web a un navegador. Por servidores web debemos tener en cuenta dos cosas:  **software del servidor y equipo** en el que el software del servidor web se ejecuta. Adicionalmente, podemos decir que un **host** puede alojar múltiples soluciones de software para servidores web.

SI hacemos un poco de historia, el desarrollo de los servidores web está vinculado al físico e informático británico [**Tim Berners-Lee**](https://es.wikipedia.org/wiki/Tim\_Berners-Lee), que en 1989 sugirió que el intercambio de información en la Organización Europea para la Investigación Nuclear debería realizarse a través de un sistema de hipertexto.

En 1990, junto con [**Robert Cailliau**](https://es.wikipedia.org/wiki/Robert\_Cailliau), presentó un proyecto llamado “[CERN httpd](https://www.w3.org/Daemon/)”, y también se creó el primer navegador web y otros elementos básicos de Internet como HTML y HTTP.

Un servidor web puede entregar contenido simultáneamente a varios PC o navegadores web. La cantidad de solicitudes (REQUESTS) y la velocidad con la que pueden ser procesadas depende, entre otras cosas, del **hardware** y la carga (**número de solicitudes**) del host. Sin embargo, la complejidad del contenido también juega un papel importante: **los contenidos web dinámicos necesitan más recursos que los contenidos estáticos**.

&#x20;En resumen, un servidor web es un servidor que satisface peticiones de equipos clientes de forma remota utilizando el llamado modelo cliente servidor.

&#x20;Por tanto, los servidores HTTP suelen almacenar por lo general páginas web, que incluyen archivos HTML, PHP, ASP, etc., así como imágenes, vídeos y audio.

### &#x20;Funcionamiento

1. Primero, el usuario escribe la URL del sitio web que quiere visitar en la barra de direcciones de su navegador web.
2. El navegador del cliente envía una solicitud al servidor web, el cual responde entregando la página HTML.
3. La página HTML puede ser **un documento estático** en el host o ser **generada de forma dinámica**,
4. Si la página web es dinámica el servidor web tiene que ejecutar un código de programa que puede ser Java, PHP, etc. antes de tramitar su respuesta. Incluso, puede ser que requiera de una DB que puede o no estar alojada en el propio servidor web.
5. El navegador interpreta la respuesta, lo que suele generar automáticamente más solicitudes al servidor como, por ejemplo, imágenes código CSS, JavaScript, etc.

### Protocolos

El código que envía el servidor al cliente es renderizado por un navegador web. Para la transmisión de datos se utiliza el protocolo HTTP (HTTPS) perteneciente a la capa de aplicación del modelo OSI.

El protocolo HTTP es el protocolo de comunicación que permite las transferencias de información en la World Wide Web que a su vez, está basado en los **protocolos de red** TCP / IP (y rara vez UDP).

HTTP ha evolucionado en múltiples versiones, muchas de las cuales son compatibles con las anteriores. El cliente le dice al servidor al principio de la petición la versión que usa, y el servidor usa la misma o una anterior en su respuesta.

### Funciones de un servidor web

Parte de las funciones que debe cumplir un servidor web son:

* **Seguridad** - Cifrado de la comunicación entre el servidor web y el cliente vía HTTPS
* **Autenticación** - Autenticación HTTP para áreas específicas de una aplicación web
* **Caching** - Almacenamiento en caché de documentos dinámicos para la respuesta eficiente de solicitudes y para evitar una sobrecarga del servidor web
* **Cookies** - Envío y procesamiento de cookies HTTP

### &#x20;Tipos de servidores web

Existen diferentes tipos de software para ejecutar un servidor web. Algunos son gratis otros de pago pero l**a condición principal a cumplir es que el software sea compatible con el SO del host**.

La mayoría de los servidores web están basados en Linux, pero también los hay que se ejecutan en Windows, con excepción de Windows IIS, que solo es ejecutable en servidores Windows.

#### &#x20;Servidor APACHE

Apache nació en 1996. Es un programa de código abierto, siendo además un software gratuito, y multiplataforma puesto que funciona sobre Windows, Linux y Unix. Es mantenido y desarrollado por la [Apache Software Foundation](https://httpd.apache.org/docs/2.4/es/).

El stack LAMP (Linux, Apache, MySQL y PHP) lo popularizó muchísimo durante el auge de las aplicaciones hechas en [PHP](http://php.net/) desde el año 2000 en adelante. Por ejemplo: XAMPP, WAMPP, MAMP.

El término <mark style="color:blue;">`Stack,`</mark> en el mundo del desarrollo de software, viene del término acuñado <mark style="color:blue;">`Stack de soluciones`</mark>  y no es mas que una colección de tecnologías que se empaquetan para formar una plataforma. Esta plataforma admite cualquier tipo de aplicación.

<mark style="color:blue;">`XAMPP`</mark> es también software libre, y contiene igualmente un sistema de gestión de bases de datos `MySQL`, el servidor web `Apache` y los intérpretes para lenguajes de script `PHP` y `Perl`. El nombre es en realidad un acrónimo:&#x20;

* X (para cualquiera de los diferentes sistemas operativos),&#x20;
* Apache, MariaDB / MySQL, PHP, Perl.
* A partir de la versión 5.6.15, XAMPP cambió la base de datos MySQL por MariaDB, un fork de MySQL con licencia GPL.

El programa se distribuye bajo licencia GNU, disponible para Windows, GNU/Linux, Solaris y Mac OS X. Es capaz de funcionar como un servidor web libre, fácil de usar y capaz de interpretar páginas dinámicas.&#x20;

&#x20;Tiene una gran desventaja y es su bajo rendimiento cuando se reciben miles de requests simultáneas de pedidos de contenido dinámico o archivos estáticos, quedando rezagado por su arcaica arquitectura versus nuevas y mejores opciones como **Nginx**.

### &#x20;Apache Tomcat

También llamado **Jakarta Tomcat** o simplemente **Tomcat** este servidor funciona como un contenedor de servlets desarrollado bajo el proyecto Jakarta de la Apache Software Foundation.

&#x20;Se trata de un servidor web de código abierto desarrollado en Java y quien quiera crear contenidos web con Java, encontrará en Apache Tomcat la solución idónea.

Un <mark style="color:blue;">`servlet`</mark> es una clase de Java que se utiliza para ampliar las capacidades de un servidor.  La palabra servlet viene de otra anterior, applet, que hace referencia a pequeños trozos de código que se ejecutan en un navegador web.

El uso más común de los servlets es generar páginas web de forma dinámica a partir de los parámetros de la petición que envíe el navegador web ([https://www.arquitecturajava.com/que-es-un-servlet/)](https://www.arquitecturajava.com/que-es-un-servlet/)

#### Nginx

En inglés se pronuncia “engine ex” y se trata de un servidor web open source y gratuito, aunque también existe una versión comercial.

Nginx creado originalmente por [Igor Sysoev](https://www.nginx.com/people/igor-sysoev/), y tuvo su primer lanzamiento público en octubre de 2004. Inicialmente Igor concibió el software como una respuesta al llamado [problema C10K](https://es.wikipedia.org/wiki/Problema\_C10k), que se refiere al problema de rendimiento de manejar 10,000 conexiones concurrentes. Esto es un problema de optimización de conexiones de red para gestionar un gran número de clientes al mismo tiempo.&#x20;

La denominación <mark style="color:blue;">`C10k`</mark> hace referencia  al problema de administrar diez mil conexiones concurrentes. Cabe notar que las conexiones concurrentes no son lo mismo que **solicitudes por segundo**, aunque son conceptos similares porque se trata de <mark style="color:blue;">administrar muchas solicitudes por segundo requiere una alta capacidad de procesamiento</mark>, mientras que un <mark style="color:blue;">alto número de conexiones concurrentes requiere administrar las conexiones de forma eficiente</mark>.

En el problema de optimización de las conexiones se requiere considerar varios factores para que un servidor web pueda gestionar muchos clientes. Entre estos factores se incluye una combinación de restricciones de SO y limitaciones del servidor web.&#x20;

Por tanto, <mark style="color:blue;">`Nginx`</mark> fue inicialmente desarrollado con la idea de superar el rendimiento ofrecido por el servidor web Apache. Al servir documentos estáticos, Nginx usa bastante menos memoria que Apache, y puede manejar alrededor de 4 veces más solicitudes por segundo.

&#x20;Este software destaca por un alto rendimiento e incluye funciones de servidor proxy reverso HTTP para reducir la carga del servidor y permitirle trabajar más rápido, así como POP3 y IMAP. Está disponible para Windows, Linux y Unix.

#### Ventajas de Nginx

* Configuración simple y potente, que permite configurarlo para integrarse nativamente con casi cualquier tecnología y lenguaje de programación moderno.&#x20;
* Ideal para despachar documentos estáticos y dinámicos.
* Poco consumo de recursos en entornos de muchas visitas simultáneas, ideal no sólo para despachar visitas de un modo rápido, sino también para evitar agregar nuevo hardware cuando no es necesario realmente.

#### Desventaja

No soporta el tipo de archivo .<mark style="color:blue;">`htaccess`</mark> clásico de Apache, aunque incluye su propio lenguaje de REWRITES.

&#x20;

### ¿Qué es un hosting?

El hosting o alojamiento web es **el espacio donde se aloja un sitio web** para que cualquiera pueda verlo en Internet.

Existen los:

* **hostings compartidos**. Quiere decir que varios hostings comparten un mismo servidor físico.
* Hosting gratuito (asumes publicidad y recursos muy limitados).
* [Hosting compartido](https://www.webempresa.com/hosting/hosting-compartido-ventajas-y-desventajas.html) (el más común en los alojamientos web).
* Hosting dedicado.

&#x20;Si contrataras un hosting compartido estarías usando los recursos de un web server junto con otros clientes. Por supuesto, los datos son privados para todos; nadie puede entrar en tu hosting ni tú podrás entrar al hosting de los demás usuarios.

#### &#x20;Ejemplos de hosting

* Gratuito: Hostinger – 000webhost
* Pago: Arsys, CDMON, OVH

#### &#x20;Servidores web vs hosting

Un servidor permite alojar contenido que luego es distribuido de diferentes formas. La más común hoy día son las páginas web, si bien que los contenidos distribuidos mediante Apps cada vez cobran más protagonismo.

&#x20;La diferencia entre servidor web y un hosting está sobre todo en el presupuesto disponible y los conocimientos para administrar o no un servicio de tipo y todo lo que eso trae consigo.

&#x20;La pregunta adecuada sería ¿para que vas a utilizar el servicio de alojamiento web? Según la respuesta y tu presupuesto, al igual que los conocimientos en administración de sistemas que tengas podrás decantarte por contratar hosting web o un servidor web.

&#x20;Por una parte tenemos los Hosting VPS, Cloud o Compartidos, pero existen otros servicios asociados a los servidores web que no necesariamente son productos que un usuario final vaya a contratar.

&#x20;

| Servidor web                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Hosting                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p>Te permiten alojar uno o pocos sitios en un servidor donde disfrutas de todos los recursos de CPU, memoria, disco y ancho de banda sin tener que compartirlos con otras cuentas de Hosting.</p><p>Por lo general su contratación es más económica que la de un Hosting compartido tradicional, pero tienen un coste más elevado en la parte de implementación y además requieren de altos conocimientos en la administración de sistemas.</p><p> </p>                                                                                 | <p>Solución adecuada para quienes solo quieren centrarse en construir su Blog, una Tienda online de productos, una página de servicios, etc., y no tienen tiempo o conocimientos para lidiar con la puesta en marcha y seguridad de un servidor web.</p><p>Te permite contratar un Plan de Hosting específico, limitado por regla general en tamaño de disco disponible, recursos de memoria, procesos y otros, pero a cambio desentenderse de la seguridad y el funcionamiento del mismo.</p><p> </p> |
| <p>·        Ventajas:</p><p>·        Administración del servidor al 100%</p><p>·        Escalabilidad inmediata bajo demanda.</p><p>·        Elección del hardware de la máquina y del sistema operativo.</p><p>·        Precios baratos y competitivos.</p><p>·        Disponibilidad completa del ancho de banda asignado al servidor.</p><p>·        Accesos total a todos los archivos del sistema.</p><p>·        Control total de los procesos que se ejecutan en la máquina.</p><p>·        IP fija o dedicada única.</p><p> </p> | <p>·        Ventajas:</p><p>·        Administración delegada por cuenta del proveedor.</p><p>·        Soporte 24/7/365 en todo lo relativo al Hosting.</p><p>·        Muchos proveedores de Hosting compartido ofrecen soporte a CMS.</p><p>·        Copias de Seguridad diarias.</p><p> </p><h4>  </h4>                                                                                                                                                                                               |
| <p>1)     Desventajas:</p><p>2)     Requiere conocimientos en Administración de Sistemas.</p><p>3)     No dispone de Soporte centrado en la Administración.</p><p>4)     Requiere altos conocimientos en seguridad en servidores.</p><p>5)     El Soporte se debe contratar por separado.</p><p>6)     Requiere la compra del software, licencias y actualizaciones.</p><p>7)     La seguridad corre por cuenta del proveedor.</p><p> </p>                                                                                               | <p>Desventajas:</p><p>·        Precios más elevados.</p><p>·        El hardware y S.O van impuestos en la contratación.</p><p>·        Ancho de banda ajustado al tipo de Hosting que se contrata.</p><p>·        Limitación en el accesos a los archivos del servidor.</p><p>·        Limitación de los procesos a ejecutar en base al Hosting contratado.</p><p>·        IP compartida (normalmente geolocalizada).</p><p> </p>                                                                      |

&#x20;

### Links

[https://recluit.com/que-es-lamp-stack/#.Y3JBz5rMJPY ](https://recluit.com/que-es-lamp-stack/#.Y3JBz5rMJPY)

#### Apache en Windows

o   [https://norfipc.com/internet/instalar-servidor-apache.html](https://norfipc.com/internet/instalar-servidor-apache.html)

o   [https://es.wikihow.com/instalar-Apache-en-Windows](https://es.wikihow.com/instalar-Apache-en-Windows)

#### Guía de instalación

* [https://httpd.apache.org/download.cgi#apache22](https://httpd.apache.org/download.cgi#apache22)

