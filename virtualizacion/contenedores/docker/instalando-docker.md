---
description: N-Tech Admin Group
---

# Instalando Docker

#### Instalar Docker en Ubuntu 20.04

Con el lanzamiento de Docker en 2013, los contenedores ganaron popularidad rápidamente. La función principal de Docker es: desarrollar, enviar y ejecutar cualquier aplicación en cualquier sistema, constituyéndose así como una alternativa flexible y capaz de ahorrar recursos frente a la emulación de componentes de hardware basada en máquinas virtuales (VM).

La virtualización de hardware tradicional se basa en iniciar diferentes sistemas operativos en el mismo sistema host (host), sin embargo con Docker (contenedor) las aplicaciones se ejecutan como procesos aislados en el mismo sistema. Por lo tanto, hablamos de virtualización basada en contenedores lo que implica que también hablamos de virtualización a nivel de sistema operativo.&#x20;

#### **Actualizar la lista de paquetes de Ubuntu**

sudo apt updates

#### **Instalar paquetes que permitan a APT descargar a través de HTTPS**

sudo apt install apt-transport-https ca-certificates curl software-properties-common

#### **Añadir la clave de GPG para el repositorio oficial de Docker. De no estar, Ubuntu no lo instala**

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

#### **Agregar el repositorio de Docker a las fuentes de APT**

sudo add-apt-repository "deb \[arch=amd64] https://download.docker.com/linux/ubuntu $(lsb\_release -cs) stable"

#### **Actualizar nuevamente los paquetes del sistema**

sudo apt update

#### **Asegurar hacer la instalación desde repositorio de Docker y no desde el predeterminado de Ubuntu**

apt-cache policy docker-ce

#### **Finalmente, instalar**

sudo apt install docker-ce

#### **Comprobar funcionamiento**

sudo systemctl status docker

#### **Ejecutar docker sin sudo**

Por defecto, el comando **docker** solo puede ser ejecutado por el usuario **root** o un usuario del grupo **docker**, el cual se crea automáticamente durante el proceso de instalación de Docker.

Si se intenta ejecutar el comando **docker** sin sudo veríamos algo como:

docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?

See 'docker run --help'

#### **Agregar usuario al grupo docker**

sudo usermod -aG docker ${USER}

Puedes visualizar el archivo /etc/group para comprobarlo

Puedes cerrar y volver a abrir el servidor para aplicarlo: su - ${USER}

#### **Confirmar que se agregó el usuario al grupo docker**

id -nG

#### **Agregar usuario diferente al grupo docker**

Si es un usuario con el que no inició sesión, se puede declarar el nombre de usuario de forma explícita escribiendo:

sudo usermod -aG docker username

#### **Sintaxis general del comando docker**

docker \[option] \[command] \[arguments]

***

### Comandos de Docker

Para **ver** todos los subcomandos disponibles:

_<mark style="color:blue;">docker</mark>_

Para ver **información** sobre Docker relacionada con todo el sistema:

_<mark style="color:blue;">docker info</mark>_

Para **verificar** si puede acceder a imágenes y descargarlas de Docker Hub:

_<mark style="color:blue;">docker run hello-world</mark>_

Los contenedores de Docker se construyen con imágenes de Docker. Por defecto, se obtienen de[ ](https://www.blogger.com/blog/post/edit/8094017497211495658/8666626486400279501)[**Docker Hub**](https://www.blogger.com/blog/post/edit/8094017497211495658/8666626486400279501), un registro de Docker gestionado por la empresa Docker que es responsable del proyecto.

Cualquiera puede alojar sus imágenes en Docker Hub, de modo que la mayoría de las aplicaciones y las distribuciones de Linux que necesitará tendrán imágenes alojadas allí.

Para **buscar** la imagen de Ubuntu:

_<mark style="color:blue;">docker search ubuntu</mark>_

La secuencia de comandos rastreará Docker Hub y mostrará una lista de todas las imágenes cuyo nombre coincida con la cadena de búsqueda.

Para **descargar** la imagen oficial de **Ubuntu** a su equipo:

_<mark style="color:blue;">docker pull ubuntu</mark>_

Para ver las **imágenes descargadas** a su PC:

_<mark style="color:blue;">docker images</mark>_

**Ejecutar** un contenedor:

_<mark style="color:blue;">docker run -it ubuntu</mark>_

Realmente ejecuta un comando en un nuevo contenedor. Este comando **run** primero crea una capa de contenedor en la que se puede escribir sobre la imagen especificada y luego la inicia con el comando especificado.

Este ejemplo, ejecuta un contenedor usando la imagen más reciente de Ubuntu. La combinación de las opciones **-i** y **-t** brindan acceso interactivo del Shell al contenedor.

Ten en cuenta el ID del contenedor en el símbolo del sistema. En mi caso es **add8ed81f423.**

Dentro del contenedor puedes ejecutar cualquier comando y no es necesario utilizar el comando **sudo**, ya que realizas las operaciones como el usuario **root**.

Para **ejecutar** un comando en un **contenedor en ejecución**.

_<mark style="color:blue;">docker exec</mark>_

Este comando solo se puede utilizar mientras se ejecuta el proceso principal del contenedor y no se reinicia si se reinicia el contenedor.

_<mark style="color:blue;">$ docker exec 270dea23c68b /etc/init.d/nginx start</mark>_

Si probamos a **instalar** una aplicación como Node.js:

_<mark style="color:blue;">apt install nodejs</mark>_

**Verificar** que está instalado el Node.js

_<mark style="color:blue;">node -v</mark>_

**Cerrar** el contenedor

_<mark style="color:blue;">exit</mark>_

**Ver** solo contenedores **activos**

_<mark style="color:blue;">docker ps</mark>_

**Ver** todos los contenedores **activos e inactivos**

_<mark style="color:blue;">docker ps -a</mark>_

Ver **último** contenedor creado

_<mark style="color:blue;">docker ps -l</mark>_

Para **iniciar** un contenedor detenido, necesitas el ID del contenedor

_<mark style="color:blue;">docker start</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**add8ed81f423**</mark>_

El contenedor se **iniciará** y podrá usar **docker ps** para ver su estado:

_<mark style="color:blue;">docker ps</mark>_

Para **detener** un contenedor en funcionamiento, seguido del ID o nombre del contenedor

_<mark style="color:blue;">docker stop</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**add8ed81f423**</mark>_

Si ya no necesitas un contenedor, lo puedes **eliminar** y usar nuevamente el ID o el nombre del contenedor.

_<mark style="color:blue;">docker rm</mark>_

Puedes utilizar el comando **docker ps -a** para encontrar el ID o nombre del contenedor asociado con la imagen **hello-world** y elimínelo.

_<mark style="color:blue;">docker rm affectionate\_swanson</mark>_

Puedes iniciar un nuevo contenedor y darle un nombre usando el conmutador&#x20;

_<mark style="color:blue;">--name</mark>_

También podrá usar el conmutador de --rm para crear un contenedor que se elimine de forma automática cuando se detenga.

Consulta el comando&#x20;

_<mark style="color:blue;">docker run help</mark>_ para obtener más información sobre estas y otras opciones.

Los contenedores pueden convertirse en imágenes que podrá usar para crear contenedores nuevos. Veamos cómo funciona esto.

Docker brinda un comando que te permite eliminar cualquier recurso: imágenes, contenedores, volúmenes y redes, que no estén asociados con un contenedor:

_<mark style="color:blue;">docker system prune</mark>_

Para eliminar adicionalmente los contenedores detenidos y todas las imágenes no utilizadas (no solo aquellas pendientes), añada el indicador `-a` al comando:

_<mark style="color:blue;">docker system prune -a</mark>_

#### Confirmar cambios que se han aplicado a una imagen de docker <a href="#h.856756hirmmz" id="h.856756hirmmz"></a>

Al iniciar una imagen de Docker, es posible crear, modificar y eliminar archivos del mismo modo que con una **máquina virtual**.

Los cambios que realices solo se aplican al contenedor sobre el que estás trabajando. De modo que podrás iniciarlo y detenerlo, pero si lo destruyes con el comando **docker rm**, los cambios se perderán por completo.

Veamos como guardar el estado de un contenedor como una nueva imagen de Docker (entiendo que al estilo de una instantánea de Virtual Box).

Dado que hemos instalado Node.js dentro del contenedor de Ubuntu, tenemos un contenedor que se ejecuta a partir de una imagen diferente de la utilizada para crearlo. Sin embargo, veremos como reutilizar este contenedor **Node.js** como base de nuevas imágenes posteriores.

Se tienen que confirmar los cambios en una nueva instancia de imagen de Docker utilizando el siguiente comando:

docker commit -m "What you did to the image" -a "Author Name" container\_id repository/new\_image\_name

donde:

* **-m** para el mensaje de confirmación que te permite a saber qué cambios realizaron,
* **-a** para especificar el autor.
* **container\_id** el id del contenedor.
* **repository** corresponde a su nombre de usuario de Docker Hub.

Por ejemplo, para el usuario **kirby**, con el ID de contenedor **add8ed81f423**, el comando sería el siguiente:

_<mark style="color:blue;">docker commit -m "added Node.js" -a "kirby" add8ed81f423 kirby/ubuntu-nodejs</mark>_

Al _confirmar - confirme -_ una imagen, la nueva se guardará a nivel local en su PC.

Si ahora listamos las imágenes de Docker de nuevo mostrará la nueva imagen, así como la anterior de la que derivó:

_<mark style="color:blue;">docker images</mark>_

* En este ejemplo **ubuntu-nodejs** es la nueva imagen, derivada de la imagen **ubuntu** existente de Docker Hub.
* La diferencia de tamaño refleja los cambios realizados. En este ejemplo, el cambio fue la instalación de **NodeJS**.
* Por ello, la próxima vez que deba ejecutar un contenedor usando Ubuntu con NodeJS preinstalado, podrá usar simplemente la nueva imagen.

Nota: También podrá crear imágenes de un Dockerfile, lo cual le permitirá automatizar la instalación de software en una nueva imagen.

En un contenedor interactivo con

_<mark style="color:blue;">$ docker run -it ubuntu</mark>_

Podemos, por ejemplo, actualizar los paquetes e instalar apache2.

Al salir del contenedor, éste se detiene pero aún se encuentra disponible para realizar una operación de **commit** con los cambios realizados y poder crear una nueva imagen ubuntu:apache2

```
$ docker commit 6cdbe0b45fcc ubuntu:apache2

se creará una nueva imagen y el TAG se llamará "apache2" 

Dado que el contenedor 6cdbe0b45fcc está cerrado podemos exportarlo como un fichero tar:

$ docker export 6cdbe0b45fcc > ubuntu-apache2.tar

En este punto puedo eliminar la imagen  del contenedor 6cdbe0b45fcc (por testear) y la recuperé con un import, esto es:

$ docker import - ubuntu-apache2 < ubuntu-apache2.tar

Al visualizar las imágenes que tengo me muestra lo siguiente:
$ docker images

REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
ubuntu-apache2       latest    06d4cdd86646   3 seconds ago   215MB
ubuntu               latest    1318b700e415   36 hours ago    72.8MB
jasonrivers/nagios   latest    bf1b57632d19   2 weeks ago     762MB
nginx                alpine    b9e2356ea1be   3 weeks ago     22.8MB
crorvick/redhat      latest    bee79eeae3be   4 years ago     361MB
```

#### Dockerfile

Es la forma de automatizar el proceso de creación de una imagen de Docker en un "manifiesto", esto es un fichero llamado Dockerfile.

Este fichero usa instrucciones para:

* definir la imagen base para el nuevo contenedor,
* las aplicaciones que deben instalarse y sus dependencias,
* los ficheros presentes en la imagen,
* los puertos accesibles desde el exterior, y
* el comando a ejecutar en el inicio del contenedor, entre otros.

Algunas instrucciones propias del dockerfile:

* **MANTAINER**: configurar datos del autor, nombre y email &#x20;
* **ENV**: configurar variables de entorno
* **ADD**: se encarga de copiar los ficheros y directorios desde una ubicación especificada y los agrega al sistema de ficheros del contenedor.&#x20;
* **COPY**: similar a ADD
* **EXPOSE**: indica los puertos en los que va a escuchar el contenedor
* **VOLUME**: nos permite utilizar una ubicación de nuestro _host_ en el contenedor. De este modo podemos almacenar permanentemente los datos.\
  Los volúmenes de los contenedores siempre son accesibles en el _host_ anfitrión, en la ubicación:

_<mark style="color:blue;">/var/lib/docker/volumes</mark>_&#x20;

* WORKDIR: directorio donde vamos a ejecutar las acciones
* SHELL, ARG, USER

**Ejemplo:**

Veamos un ejemplo de dockerfile. El fichero se llama así tal cual (en este caso):

```
FROM ubuntu:20.04
FROM ubuntu:latest
RUN apt-get update -y
RUN apt-get install nginx -y
CMD ["bash"]
```

Donde:

FROM crea una capa a partir de la imagen de Docker ubuntu: 20.04. La imagen base de la cual partiremos.\
COPY agrega archivos del directorio actual de su cliente Docker.\
RUN crea su aplicación con make.\
CMD especifica qué comando ejecutar dentro del contenedor.

Para poder crear esta imagen:

$ docker build -t "test-ub-nginx:dockerfile" .

En pantalla podemos verificar la ejecución de las instrucciones y finalmente con el comando docker images comprobar que se ha creado la imagen a partir del dockerfile. Esto es:

$ docker images\


```
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
testnginx            dockfile  6ebe631b1c62   22 seconds ago   162MB
ubuntu-apache2       latest    06d4cdd86646   3  seconds ago   215MB
ubuntu               latest    1318b700e415   36 hours ago     72.8MB
jasonrivers/nagios   latest    bf1b57632d19   2  weeks ago     762MB
nginx                alpine    b9e2356ea1be   3  weeks ago     22.8MB
crorvick/redhat      latest    bee79eeae3be   4  years ago     361MB
```

\
Ya podemos crear un contenedor a partir de la imagen creada.

#### Introducir imágenes de Docker en un repositorio

Para introducir una imagen a Docker Hub o a cualquier otro registro de Docker, debes tener una cuenta en el sistema.

Nota: Para aprender a crear su propio registro privado de Docker, consulta: **Configurar un registro de Docker privado**.

Por tanto, supongamos que tienes la cuenta en el sistema. Entonces, para introducir su imagen, primero inicia sesión en Docker Hub.

docker login -u docker-registry-username

Te solicitará autenticarte usando la contraseña de Docker Hub.



### Docker compose&#x20;

<mark style="color:red;">investigar</mark>&#x20;
