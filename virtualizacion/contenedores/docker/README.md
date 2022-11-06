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

LXC y Docker son dos sistemas de contenedores. LXC es un tipo de contenedor de sistema lo que significa que todos los contenedores creados con LXC necesitan un sistema operativo propio para funcionar, al contrario de Docker, que utiliza el sistema operativo del sistema anfitrión. Lo bueno de los contenedores de sistema o LXC es que podemos tener en un solo contenedor diferentes aplicaciones.

<figure><img src="https://lh4.googleusercontent.com/pzwLC7_9fFiyRt1DtfsrSYyBdOJ8BSH8qtTLlBkKCK9nr_O5eYpxZO8EFP9km4CVhLgg0VB4HJSYGNITGaiKhLEUwD16iYltCnXE59ljX2NWiDagMTxRRn3tFYL8yHb9Ih9JnqjvzCYDrcQxgsfWBpLr5oa4EoMBpFqVHNmVnf90kky8RqCPtBIU" alt=""><figcaption></figcaption></figure>

#### Docker Compose

Se trata de una utilidad de Docker que nos ayuda a la hora de crear múltiples contenedores Docker al mismo tiempo.&#x20;

A diferencia de Docker Cli este no hay que crearlo directamente en la terminal, sino que se crea un archivo .yml que es más fácil de ser modificado y configurado que en la propia terminal.&#x20;

Una vez creado solo se tiene que ejecutar un comando por terminal que comprobará el archivo e instalará y configurará lo que le hemos descrito en el mismo.

![](<../../../.gitbook/assets/image (177).png>)



<mark style="color:red;">To be continued ...</mark>

### Links

* [https://docs.docker.com/engine/reference/commandline/compose/](https://docs.docker.com/engine/reference/commandline/compose/)
* [https://docs.docker.com/compose/reference/](https://docs.docker.com/compose/reference/)

