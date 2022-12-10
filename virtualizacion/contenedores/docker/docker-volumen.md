# Docker volumen

Es el mecanismo preferido para conservar los datos generados y utilizados por los contenedores de Docker. Depende de la estructura de los directorios y del propio <mark style="color:blue;">`SO`</mark> del equipo host. Docker administra los volúmenes y tienen ventajas que son:

* Los volúmenes son más fáciles de respaldar o migrar.&#x20;
* Se puede administrar volúmenes mediante los comandos de CLI de Docker o la API de Docker.&#x20;
* Funcionan en contenedores de Linux y Windows.&#x20;
* Se pueden compartir de forma más segura entre varios contenedores.&#x20;
* Los controladores de volumen permiten almacenar volúmenes en hosts remotos o proveedores de la nube, cifrar el contenido de los volúmenes o agregar otras funciones.&#x20;
* Los nuevos volúmenes pueden tener su contenido rellenado previamente por un contenedor.&#x20;
* Suelen ser una mejor opción que los datos persistentes en la capa de escritura de un contenedor, porque un volumen no aumenta el tamaño de los contenedores que lo utilizan y el contenido del volumen existe fuera del ciclo de vida de un contenedor determinado.

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>Tomado de <a href="https://docs.docker.com/storage/volumes/">https://docs.docker.com/storage/volumes/</a></p></figcaption></figure>





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
