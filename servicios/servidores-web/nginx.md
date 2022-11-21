# Nginx

Es un software libre y de código abierto bajo licencia BSD con una versión comercial distribuida bajo el nombre de Nginx. Su creador es [Igor Sysoev.](https://en.wikipedia.org/wiki/Igor\_Sysoev)&#x20;

Se trata de un servidor web ligero que sirve de proxy para protocolos de correo electrónico: IMAP - POP3 y es multiplataforma, con lo cual corre en sistemas tipo Unix: GNU/Linux, BSD, Solaris, Mac OS, Windows.

El sistema es usado por sitios web como: Wordpress, Netflix, GitHub, SourceForge, entre otros.

Siendo un servidor web open source ofrece contenido estático de un sitio web de una forma rápida y fácil de configurar. También ofrece:&#x20;

* Recursos de equilibrio de carga, proxy inverso y de streaming.&#x20;
* Permite gestionar miles de conexiones simultáneas.&#x20;
* Brinda mayor velocidad y escalabilidad.

[W3Techs ](https://w3techs.com/technologies/comparison/ws-apache,ws-microsoftiis,ws-nginx)nos brinda datos de comparación estadística del uso de los servidores web y NGinx ocupa ya el segundo lugar, detrás de Apache y el IIS de Microsoft.&#x20;

### Algunos aspectos del funcionamiento de NGINX

\
Si deseas un servidor web que no tengas que preocuparse por la cantidad de conexiones simultáneas realizadas al [sitio web](https://rockcontent.com/es/blog/como-crear-un-sitio-web/), debes conocer NGINX.

Por cierto, esta es solo una de sus muchas características, ya que también ofrece recursos como **el equilibrador de carga HTTP, el proxy inverso y más.**

Según los datos de la comparación estadística sobre el uso de servidores web realizada por el sitio web [W3Techs](https://w3techs.com/technologies/comparison/ws-apache,ws-microsoftiis,ws-nginx), NGINX ocupa el segundo lugar, con un 30,7% de los sitios que utilizan esta tecnología.

[Apache](https://rockcontent.com/es/blog/apache/) tiene 43.1% y Microsoft IIS, 8%. Es una posición excelente, ya que ha estado en el mercado por menos tiempo que las otras opciones.

Para que comprendas por qué NGINX sigue creciendo y sepas como crear un sitio web que procese múltiples conexiones simultáneas, hicimos este artículo con los siguientes temas:

* **¿Qué es NGINX?**
* **¿Cuándo llegó y quién lo lanzó?**
* **¿Cómo funciona?**
* **¿Cuáles son sus características?**
* **¿Por qué usar el servidor web NGINX?**
* **¿Cuáles son las diferencias entre NGINX y Apache**;

¿Quieres saber más sobre esta tecnología? ¡Sigue leyendo!

### ¿Qué es NGINX?

NGINX es un servidor web open source, de alta performance. Cuenta con una arquitectura avanzada basada en eventos — EBA, o Event-Based Architecture. Esta característica permite numerosas conexiones simultáneas, lo que proporciona **más velocidad y escalabilidad**.&#x20;

NGINX entrega el contenido estático del sitio web rápidamente, es fácil de configurar y tiene un bajo consumo de recursos.

Debido a todas estas características, el servidor es utilizado por grandes compañías como Microsoft, IBM, Google, [WordPress.org](https://rockcontent.com/es/blog/tutorial-de-wordpress/), entre otras.

### ¿Cuándo llegó NGINX y quién lo lanzó?

Su desarrollador fue el ingeniero de software Igor Sysoev, quien escribió NGINX en lenguaje C en 2002. La pronunciación correcta del nombre del servidor es "Engine-X" y su primera versión pública fue lanzada en 2004.

El desafío, conocido como C10K, era crear un servidor web que pudiera **manejar 10 mil conexiones simultáneamente** para entregar un producto que pudiera trabajar con el conjunto de referencia para la web moderna, sin embargo, con la [transformación digital](https://rockcontent.com/es/blog/transformacion-digital/), ese número se ha vuelto aún mayor.

### Características

NGINX tiene una arquitectura modular extensible que facilita la extensión de recursos para aquellos que desean cambiar su código fuente.

El módulo principal es responsable de manejar la conexión y, además, hay una serie de módulos para diferentes tipos de procesamiento.

Algunas de las características fundamentales son:

* **Balanceo de carga**:  Se trata de un recurso importante para quienes necesitan de un sitio web con alta disponibilidad, ya que permite la distribución de solicitudes de servicio entre servidores. De esa manera, cuando hay un aumento en las solicitudes al servidor, como un aumento en el tráfico, NGINX puede dirigir el flujo a otros servidores que están en el archivo de configuración.
* **Proxy inverso**: Actúa como un servidor intermediario entre las computadoras en una red y el servidor web. Se utiliza como caché de página para ahorrar recursos de banda y acelerar la carga. El proxy inverso es un servidor web que recibe solicitudes de conexión y administra lo que se requerirá en el servidor principal o verifica si la solicitud ya está disponible en caché.
* **Streaming**: Ofrece un módulo nativo para streaming y permite una serie de configuraciones sobre cómo el servidor maneja el contenido MP4 y FLV, como el tamaño del buffer utilizado, el tiempo de timeout, etc.&#x20;

### ¿Cómo funciona NGINX?

En servidores web como [Apache](https://httpd.apache.org), las solicitudes web funcionan de manera individual:  un usuario solicita una página a través del protocolo HTTP o HTTPS, se procesa y se devuelve el resultado. Este proceso se realiza desde el servidor para cada solicitud.

Sin embargo, NGINX funciona basado en eventos, esto es: en lugar de hacer una solicitud directa al servidor, éste ejecuta un proceso maestro que se denomina <mark style="color:blue;">`worker`</mark>, así como varios procesos de trabajo llamados <mark style="color:blue;">`conexiones worker`</mark>.&#x20;

En el momento que haya una solicitud de procesamiento, se realizan las <mark style="color:blue;">conexiones worker</mark>, que realizan la solicitud al proceso maestro, que a su vez procesa y devuelve el resultado. Esto permite poder manejar numerosas conexiones simultáneas, dado que cada <mark style="color:blue;">`conexión worker`</mark> es capaz de procesar <mark style="color:blue;">1024</mark> solicitudes.

### ¿Por qué utilizar  Nginx?

Entre las características que ofrece NGinx como servidor web y que lo hacen "apetecible" tenemos:

* **Velocidad**: Dado que su arquitectura se basa en eventos, las solicitudes al servidor se realizan más rápido, dado que hay un mejor uso de los recursos de memoria y CPU. Por otra parte, ofrece un excelente performance al poner a disposición archivos estáticos, como documentos, imágenes, archivos HTML, etc.
* **Escalabilidad**: Ofrece recursos como el equilibrio de carga, con lo cual el servidor permite una escalada rápida de solicitudes en diferentes situaciones, lo que lo convierte enuna excelente alternativa para usar en aplicaciones en la nube.
* **Compatibilidad**: Ofrece compatibilidad con diversas aplicaciones web como WordPress, Joomla, Python, etc.
* **Fácil configuración**: Funciona según las políticas que deben especificarse en el archivo de configuración.

### NGinX vs Apache



<table><thead><tr><th>NGINX</th><th>APACHE</th><th data-hidden></th></tr></thead><tbody><tr><td>Funciona en Unix, Linux y WIndows, pero tiene un funcionamiento inferior en éste último.</td><td>Funciona en Unix, Linux y Windows.</td><td></td></tr><tr><td>Configuración centralizada en el archivo nginx.conf y los módulos se cargan de manera dinámica.</td><td>Configuración descentralizada: utiliza el archivo .htaccess y la carga de los módulos se realiza durante la ejecución.</td><td></td></tr><tr><td>Capacidad de operar con miles de conexiones simultáneas, al doble de la velocidad requerida en Apache</td><td>Sigue siendo un servidotr web popular</td><td></td></tr><tr><td>Consume menos memoria para el contenido estático.</td><td>Mayor flexibilidad, dado que admite la personalización de conexiones a través de .htaccess.</td><td></td></tr><tr><td>Ofrece equilibrio de carga, proxy inverso</td><td></td><td></td></tr></tbody></table>

### Instalación

Está pensado para ser instalado en cualquier servidor dedicado, estructura cloud o VPS, ya que es necesario tener acceso como administrador para poder llevar a cabo la instalación de este servidor web.&#x20;

Aquí veremos el proceso de instalación de nginx en un servidor de Ubuntu, aunque el proceso de instalación en otro sistema operativo es muy parecido.&#x20;

Para instalar nginx tenemos que hacer:

```
sudo apt-get install nginx 
```

Cuando se haya completado la instalación, tenemos que arrancar el servicio. Para ello ejecutamos:&#x20;

```
sudo service nginx start
```

Para comprobar que el servidor está funcionando, en nuestro navegador podemos escribir la dirección “localhost”. Si todo está correcto deberíamos ver una página de bienvenida similar a la siguiente.\


<figure><img src="../../.gitbook/assets/image (1) (5).png" alt=""><figcaption></figcaption></figure>

### Configuración

Una vez instalado nginx ya podremos utilizarlo pero podemos mejorar su rendimiento editando el archivo nginx.conf que se encuentra en <mark style="color:blue;">`/etc/nginx`</mark>.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Igualmente tienes la posibilidad de acceder al manual de nginx desde:

```
man nginx
```

Directivas que podemos modificar:

* **Worker\_processes**: el valor indicado en esta directiva podrá determinar el número máximo de procesos simultáneos que a gestionar por el servidor web. Para ello, es necesario conocer el número de procesos que puede gestionar nuestro equipo. \
  Podemos hacer uso de la siguiente instrucción y el valor que nos devuelva, sería el que indicaremos en esta directiva:

```
grep processor /proc/cpuinfo | wc –l 
```

* **Directiva worker\_connections**: el valor que determina el número máximo de conexiones que puede tener el sitio. Si nuestro sitio tiene un elevado número de visitas, es recomendable aumentar este valor. Por defecto viene configurado con el valor de 768, pero se puede modificar y poner un valor superior, por ejemplo 1024.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* **Directiva keepalive\_timeout**: Se trata de  una directiva que se utiliza para mantener la conexión abierta durante un cierto número de solicitudes al servidor o hasta que expire el período de tiempo de espera de la solicitud. Según los desarrolladores de Nginx, 10 000 conexiones inactivas usarían solo 2,5 MB de memoria, lo que demuestra que Nginx es excepcionalmente bueno para manejar conexiones inactivas debido a las conexiones **keepalive**. \
  Adicionalmente, tiene gran influencia en la percepción del tiempo de carga por parte del usuario final y se puede utilizar esta opción para optimizar el tiempo de carga de un sitio web.\
  \
  Dado que la creación de nuevas conexiones TCP puede consumir gran cantidad de recursos, como memoria y uso de CPU el hecho de mantener viva su conexión en Nginx  permite reducir este consumo. Siendo una de las razones por la que se recomienda el uso de keepalive para las conexiones HTTPS. \
  Habilitar el keepalive puede ayudarlo a mejorar la experiencia del usuario y el rendimiento del sitio web. Permite que el navegador cargue el contenido de la página con una sola conexión TCP. \
  Otro beneficio es que mejora la velocidad de la página web debido a su capacidad para entregar varios archivos a través de la misma conexión, lo que reduce la latencia y acelera la carga de las páginas web.

```
keepalive_timeout 10;
```

### Módulos&#x20;

Nginx permite ampliar su funcionalidad por medio del uso de módulos. Algunos de los módulos más importantes que nos podemos encontrar son:&#x20;

* **HTTP Referer**. Filtra peticiones recibidas en función de la cabecera Referer.&#x20;
* **HTTP Limit Zone**. Limita el número de conexiones simultáneas desde un mismo cliente.&#x20;
* **User ID**. Proporciona cookies identificativas.&#x20;
* **FLV**. Permite reproducir vídeo en streaming.&#x20;
* **Perl**. Módulo que permite ejecutar Perl directamente dentro de Nginx.
* **WebDAV**. Ofrece soporte para [WebDAV](https://es.wikipedia.org/wiki/WebDAV).&#x20;
* **Secure Link**. Este módulo ofrece la posibilidad de proteger páginas mediante clave secreta.&#x20;
* **XSLT**. Funcionalidad que permite el post-procesamiento de páginas mediante [XSLT](https://www.w3schools.com/xml/xsl\_intro.asp)

### Links

* [https://linuxhint.com/what-is-keepalive-in-nginx/ ](https://linuxhint.com/what-is-keepalive-in-nginx/)
* [https://www.nginx.com/blog/avoiding-top-10-nginx-configuration-mistakes/](https://www.nginx.com/blog/avoiding-top-10-nginx-configuration-mistakes/)
* [https://www.redeszone.net/tutoriales/internet/webdav-que-es-configuracion/ ](https://www.redeszone.net/tutoriales/internet/webdav-que-es-configuracion/)
* [https://www.w3schools.com/xml/xsl\_intro.asp](https://www.w3schools.com/xml/xsl\_intro.asp)



### Probando un sitio web

Si quieres probar el funcionamiento de nginx puedes crear o subir un sitio web en la dirección&#x20;

```
/var/www/html/example.com 
```

[https://enclaveinformatico.com/nginx-instalando-y-configurando-un-servidor-web/](https://enclaveinformatico.com/nginx-instalando-y-configurando-un-servidor-web/)

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



