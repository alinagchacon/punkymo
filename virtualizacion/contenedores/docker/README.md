---
description: N-Tech Admin Group
---

# Docker

La función principal de Docker es desarrollar, enviar y ejecutar cualquier aplicación en cualquier sistema, constituyéndose así como una alternativa flexible y capaz de ahorrar recursos frente a la emulación de componentes de hardware basada en máquinas virtuales (VM).

No es lo mismo Docker que un contenedor Linux. Docker se desarrolló a partir de la tecnología LXC, lo que la mayoría de las personas asocia con contenedores de Linux "tradicionales", aunque desde entonces se ha alejado de esa dependencia. LXC era útil como virtualización ligera, pero no ofrecía una buena experiencia al desarrollador ni al usuario.

#### Ventajas

* Modularidad
* Control de versiones de imágenes y capas
* Restauración
* Implementación rápida

#### ¿Qué diferencia habría entre LXC y Docker?

<mark style="color:blue;">LXC</mark> y <mark style="color:blue;">`Docker`</mark> son dos sistemas de contenedores. LXC es un tipo de contenedor de sistema lo que significa que todos los contenedores creados con LXC necesitan un sistema operativo propio para funcionar, al contrario de Docker, que utiliza el sistema operativo del sistema anfitrión. Lo bueno de los contenedores de sistema o LXC es que podemos tener en un solo contenedor diferentes aplicaciones.

<figure><img src="https://lh4.googleusercontent.com/pzwLC7_9fFiyRt1DtfsrSYyBdOJ8BSH8qtTLlBkKCK9nr_O5eYpxZO8EFP9km4CVhLgg0VB4HJSYGNITGaiKhLEUwD16iYltCnXE59ljX2NWiDagMTxRRn3tFYL8yHb9Ih9JnqjvzCYDrcQxgsfWBpLr5oa4EoMBpFqVHNmVnf90kky8RqCPtBIU" alt=""><figcaption></figcaption></figure>

Se puede decir que un contenedor permite a un desarrollador empaquetar una aplicación con todos los componentes que necesita (bibliotecas y dependencias), así como poder utilizarlo como si fuera un solo paquete.&#x20;

Más aún, **los contenedores funcionan como una máquina virtual ligera pero con su propio sistema de ficheros, su propio espacio de usuarios y procesos o sus propias interfaces de red**.

Entre las características que tiene encontramos que:

Veamos más a fondo sus ventajas y características:

**Es portable**. Los contenedores se ejecutan por el [`demonio`](https://colaboratorio.net/glosario/demonio/) (Docker Engine) que es fácil de instalar en distribuciones [<mark style="color:blue;">`GNU`</mark>](https://colaboratorio.net/glosario/gnu/)<mark style="color:blue;">`/`</mark>[<mark style="color:blue;">`Linux`</mark>](https://colaboratorio.net/glosario/1989/).&#x20;

**Uso**: un contenedor ejecuta una imagen de Docker, que es una representación del sistema de ficheros y otros datos que el contenedor necesita para su ejecución. Una vez que se ha generado la imagen del contenedor,  la misma podrá ser ejecutada por cualquier Docker Engine.

**Inmutable.** Si hablamos de aplicaciones compuestas por el [`código fuente`,](https://colaboratorio.net/glosario/codigo-fuente/) las <mark style="color:blue;">`librerías`</mark> del <mark style="color:blue;">`SO`</mark> y del <mark style="color:blue;">`lenguaje de programación`</mark> necesarios para ejecutar dicho código, y que además, dependen a su vez del <mark style="color:blue;">`SO`</mark> donde se ejecutará cabría preguntarnos  si nos funcionará la aplicación en cualquier <mark style="color:blue;">`SO`</mark>.  Aquí viene en nuestra ayuda la idea de contenedores puesto que el proceso de instalación de dependencias de Docker no depende del <mark style="color:blue;">`SO`</mark>, dado que éste se realiza a la hora de generar una imagen del contenedor.&#x20;

Cuando se genera una vez una imagen, la misma puede ser ejecutada las veces que sean necesarias, y siempre se ejecutará con la misma versión del código fuente y sus dependencias, por lo que podemos decir que es inmutable.&#x20;

**Ligero**. Todos los contenedores que podemos tener en funcionamiento en una misma máquina, no importa si es virtual o física, comparten el mismo <mark style="color:blue;">`SO`</mark>, sin embargo cada contenedor es un proceso independiente, que tiene su propio sistema de ficheros y sus propio espacio de archivos.

Esta característica hace que la ejecución de contenedores sea mucho más ligera que otros modos de virtualización. Por ejemplo:&#x20;

<mark style="color:blue;">`VirtualBox`</mark>  es capaz de hacer funcionar en un PC varias <mark style="color:blue;">`VM`</mark>. En cambio, en el mismo PC podemos hacer funcionar cientos de contenedores sin causar problemas de rendimiento.

**Seguridad.** Los contenedores son compatibles con los principales sistemas de seguridad a nivel de kernel. También ofrece <mark style="color:blue;">`grupos de control de acceso`</mark> al demonio que controla la virtualización, restringiendo posibles cambios que puedan hacerse en el sistema.



### Links

* [https://docs.docker.com/engine/reference/commandline/compose/](https://docs.docker.com/engine/reference/commandline/compose/)
* [https://docs.docker.com/compose/reference/](https://docs.docker.com/compose/reference/)
* [https://docs.docker.com/storage/volumes/](https://docs.docker.com/storage/volumes/)
* [https://colaboratorio.net/davidochobits/sysadmin/2017/docker-una-guia-no-convencional/](https://colaboratorio.net/davidochobits/sysadmin/2017/docker-una-guia-no-convencional/)
* [https://keepcoding.io/blog/que-es-docker-compose/](https://keepcoding.io/blog/que-es-docker-compose/)
* [https://www.ionos.es/digitalguide/servidores/know-how/docker-container-volumes/](https://www.ionos.es/digitalguide/servidores/know-how/docker-container-volumes/)



