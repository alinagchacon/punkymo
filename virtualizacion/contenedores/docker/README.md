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

**Es portable**. Los contenedores se ejecutan por el [`demonio`](https://colaboratorio.net/glosario/demonio/) `` (Docker Engine) que es fácil de instalar en distribuciones <mark style="color:blue;">``</mark> [<mark style="color:blue;">`GNU`</mark>](https://colaboratorio.net/glosario/gnu/)<mark style="color:blue;">`/`</mark>[<mark style="color:blue;">`Linux`</mark>](https://colaboratorio.net/glosario/1989/).&#x20;

**Uso**: un contenedor ejecuta una imagen de Docker, que es una representación del sistema de ficheros y otros datos que el contenedor necesita para su ejecución. Una vez que se ha generado la imagen del contenedor,  la misma podrá ser ejecutada por cualquier Docker Engine.

**Inmutable.** Si hablamos de aplicaciones compuestas por el [`código fuente`,](https://colaboratorio.net/glosario/codigo-fuente/) las <mark style="color:blue;">`librerías`</mark> del <mark style="color:blue;">`SO`</mark> y del <mark style="color:blue;">`lenguaje de programación`</mark> necesarios para ejecutar dicho código, y que además, dependen a su vez del <mark style="color:blue;">`SO`</mark> donde se ejecutará cabría preguntarnos  si nos funcionará la aplicación en cualquier <mark style="color:blue;">`SO`</mark>.  Aquí viene en nuestra ayuda la idea de contenedores puesto que el proceso de instalación de dependencias de Docker no depende del <mark style="color:blue;">`SO`</mark>, dado que éste se realiza a la hora de generar una imagen del contenedor.&#x20;

Cuando se genera una vez una imagen, la misma puede ser ejecutada las veces que sean necesarias, y siempre se ejecutará con la misma versión del código fuente y sus dependencias, por lo que podemos decir que es inmutable.&#x20;

**Ligero**. Todos los contenedores que podemos tener en funcionamiento en una misma máquina, no importa si es virtual o física, comparten el mismo <mark style="color:blue;">`SO`</mark>, sin embargo cada contenedor es un proceso independiente, que tiene su propio sistema de ficheros y sus propio espacio de archivos.

Esta característica hace que la ejecución de contenedores sea mucho más ligera que otros modos de virtualización. Por ejemplo:&#x20;

<mark style="color:blue;">`VirtualBox`</mark> **** es capaz de hacer funcionar en un PC varias <mark style="color:blue;">`VM`</mark>. En cambio, en el mismo PC podemos hacer funcionar cientos de contenedores sin causar problemas de rendimiento.

**Seguridad.** Los contenedores son compatibles con los principales sistemas de seguridad a nivel de kernel. También ofrece <mark style="color:blue;">`grupos de control de acceso`</mark> al demonio que controla la virtualización, restringiendo posibles cambios que puedan hacerse en el sistema.



### Volumen&#x20;

El mecanismo preferido para conservar los datos generados y utilizados por los contenedores de Docker. Depende de la estructura de los directorios y del propio <mark style="color:blue;">`SO`</mark> del equipo host. Docker administra los volúmenes y tienen ventajas que son:

* Los volúmenes son más fáciles de respaldar o migrar.&#x20;
* Se puede administrar volúmenes mediante los comandos de CLI de Docker o la API de Docker.&#x20;
* Funcionan en contenedores de Linux y Windows.&#x20;
* Se pueden compartir de forma más segura entre varios contenedores.&#x20;
* Los controladores de volumen permiten almacenar volúmenes en hosts remotos o proveedores de la nube, cifrar el contenido de los volúmenes o agregar otras funciones.&#x20;
* Los nuevos volúmenes pueden tener su contenido rellenado previamente por un contenedor.&#x20;
* Suelen ser una mejor opción que los datos persistentes en la capa de escritura de un contenedor, porque un volumen no aumenta el tamaño de los contenedores que lo utilizan y el contenido del volumen existe fuera del ciclo de vida de un contenedor determinado.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>Tomado de <a href="https://docs.docker.com/storage/volumes/">https://docs.docker.com/storage/volumes/</a></p></figcaption></figure>

#### Funcionamiento general

A pesar de todo esto, si queremos entender los volúmenes de Docker, echémosle un vistazo general al funcionamiento del sistema de archivos de Docker.

* Una imagen de Docker consta de varias capas de solo lectura.&#x20;
* Al iniciarse una imagen desde un contenedor, Docker añade una nueva capa editable en la parte superior del sistema.&#x20;
* El sistema de archivos de Docker se llama **Union File System** (UnionFS).
* Cada vez que se modifica un archivo, Docker crea una copia de este mismo incluyendo las capas de solo lectura hasta la capa más alta, la que sí se puede editar.&#x20;
* De esta manera queda una copia del archivo original (de solo lectura) sin cambios.
* Al eliminar un contenedor, se pierde la capa más alta, la que tiene permisos de escritura. Esto significa que todos los cambios realizados después de iniciar el contenedor se pierden.

Como ya hemos dicho, un volumen de contenedor permite conservar los datos, aunque se elimine el Docker container. Los volúmenes también permiten un intercambio práctico de datos entre el host y el container. Por tanto, crear un volumen de Docker es una buena solución para poder:

* Transferir datos a un contenedor de Docker
* Guardar los datos de un contenedor de Docker
* Intercambiar datos entre contenedores de Docker

Adicionalmente:&#x20;

* Los volúmenes de Docker existen fuera del <mark style="color:blue;">`Union File System`</mark> con su acceso de solo lectura y la capa de escritura.&#x20;
* El volumen es una carpeta compartida entre el contenedor y la máquina host.&#x20;
* Los volúmenes también pueden ser compartidos entre contenedores.

### Manipulando volúmenes en Docker

Un volumen de contenedor se encuentra fuera del propio contenedor en el equipo host. Fuera del container, el volumen se comporta como una **carpeta** en la que se pueden almacenar datos y desde la que se pueden recuperar datos. Se trata del <mark style="color:blue;">`mount point`</mark> de un directorio en el host.

Tenemos  varias formas de crear y gestionar volúmenes en Docker  y cada método tiene sus propias ventajas e inconvenientes.

El comando _docker volume create_ crea un volumen identificado con un nombre. El nombre te permite encontrar fácilmente los Docker volumes y poder asignarlos a los contenedores específicos.

El siguiente comando se utiliza para **crear y nombrar** un volumen:

```
sudo docker volume create - - name [volume name]
```

Si queremos **crear un container que utilice un volumen ya creado**, podemos añadir el siguiente parámetro al comando _docker run_:

```
docker run -v [volume name]:[container directory]
```

Para **ejecutar** un contenedor **desde una imagen** de Ubuntu denominada _my-test-volume_ y asignar el **data-volume** al directorio o el _data_ al container, el comando sería:

```
sudo docker run -it --name my-test-volume -v data-volume:/data ubuntu /bin/bash
```

Para **listar** los volúmenes de Docker que tenemos, utilizamos el siguiente comando:

```
sudo docker volume ls
```

Este comando genera la **lista** de todos los volúmenes de Docker creados en el host.

Si queremos **inspeccionar** un volumen en concreto, utilizamos el siguiente comando:

```
sudo docker volume inspect [volume name]
```

Este comando te **brinda información sobre el volumen especificado**. Incluye el mount point y el directorio en el sistema host a través del cual se puede acceder al volumen de Docker. Si queremos obtener más información sobre el volumen  <mark style="color:blue;">`MyData`</mark> (por ejemplo) que hemos creado, entonces el comando a utilizar sería:

```
sudo docker volume inspect data-volume
```

Para **eliminar** un volumen creado de este modo, podemos utilizar el siguiente comando:

```
sudo docker volume rm [volume name]
```

No se puede eliminar un volumen si está siendo utilizado por un contenedor existente. Con lo cual, **antes de eliminar el volumen**, debemos detener y eliminar el contenedor. Para ello podemos utilizar los comandos:

```
sudo docker stop [container name or ID]
sudo docker rm [container name or ID]
```

Para eliminar el volumen denominado <mark style="color:blue;">`MyData`</mark>, primero detenemos y eliminamos el container que lo utiliza con my-test-volume:

```
sudo docker stop my-volume-test
sudo docker rm my-volume-test
```

El data-volume puede ser **borrado** con el siguiente comando:

```
sudo docker volume rm data-volume
```

### Links

* [https://docs.docker.com/engine/reference/commandline/compose/](https://docs.docker.com/engine/reference/commandline/compose/)
* [https://docs.docker.com/compose/reference/](https://docs.docker.com/compose/reference/)
* [https://docs.docker.com/storage/volumes/](https://docs.docker.com/storage/volumes/)
* [https://colaboratorio.net/davidochobits/sysadmin/2017/docker-una-guia-no-convencional/](https://colaboratorio.net/davidochobits/sysadmin/2017/docker-una-guia-no-convencional/)
* [https://keepcoding.io/blog/que-es-docker-compose/](https://keepcoding.io/blog/que-es-docker-compose/)
* [https://www.ionos.es/digitalguide/servidores/know-how/docker-container-volumes/](https://www.ionos.es/digitalguide/servidores/know-how/docker-container-volumes/)



