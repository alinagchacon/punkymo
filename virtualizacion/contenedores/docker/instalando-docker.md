---
description: N-Tech Admin Group
---

# Instalando Docker

#### Instalar Docker en Ubuntu 20.04

Con el lanzamiento de Docker en 2013, los contenedores ganaron popularidad rápidamente. La función principal de Docker es: desarrollar, enviar y ejecutar cualquier aplicación en cualquier sistema, constituyéndose así como una alternativa flexible y capaz de ahorrar recursos frente a la emulación de componentes de hardware basada en máquinas virtuales (VM).

La virtualización de hardware tradicional se basa en iniciar diferentes sistemas operativos en el mismo sistema host (host), sin embargo con Docker (contenedor) las aplicaciones se ejecutan como procesos aislados en el mismo sistema. Por lo tanto, hablamos de virtualización basada en contenedores lo que implica que también hablamos de virtualización a nivel de sistema operativo.&#x20;

#### **Actualizar la lista de paquetes de Ubuntu**

```
sudo apt updates
```

#### **Instalar paquetes que permitan a APT descargar a través de HTTPS**

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

#### **Añadir la clave de GPG para el repositorio oficial de Docker. De no estar, Ubuntu no lo instala**

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

**Agregar el repositorio de Docker a las fuentes de APT**

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

#### **Actualizar nuevamente los paquetes del sistema**

```
sudo apt update
```

#### **Asegurar hacer la instalación desde repositorio de Docker y no desde el predeterminado de Ubuntu**

```
apt-cache policy docker-ce
```

#### **Finalmente, instalar**

```
sudo apt install docker-ce
```

#### **Comprobar funcionamiento**

```
sudo systemctl status docker
```

#### **Ejecutar docker sin sudo**

Por defecto, el comando **docker** solo puede ser ejecutado por el usuario **root** o un usuario del grupo **docker**, el cual se crea automáticamente durante el proceso de instalación de Docker.

Si se intenta ejecutar el comando **docker** sin sudo veríamos algo como:

```
docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?
See 'docker run --help'
```

#### **Agregar usuario al grupo docker**

```
sudo usermod -aG docker ${USER}
```

Puedes visualizar el archivo <mark style="color:blue;">`/etc/group`</mark> para comprobarlo

Puedes cerrar y volver a abrir el servidor para aplicarlo:&#x20;

```
su - ${USER}
```

#### **Confirmar que se agregó el usuario al grupo docker**

```
id -nG
```

#### **Agregar usuario diferente al grupo docker**

Si es un usuario con el que no inició sesión, se puede declarar el nombre de usuario de forma explícita escribiendo:

```
sudo usermod -aG docker username
```

#### **Sintaxis general del comando docker**

docker \[option] \[command] \[arguments]

***

### Comandos de Docker

Para **ver** todos los subcomandos disponibles:

```
docker
```

Para ver **información** sobre Docker relacionada con todo el sistema:

```
docker info
```

Para **verificar** si puede acceder a imágenes y descargarlas de Docker Hub:

```
docker run hello-world
```

Los contenedores de Docker se construyen con imágenes de Docker. Por defecto, se obtienen de[ ](https://www.blogger.com/blog/post/edit/8094017497211495658/8666626486400279501)[**Docker Hub**](https://www.blogger.com/blog/post/edit/8094017497211495658/8666626486400279501), un registro de Docker gestionado por la empresa Docker que es responsable del proyecto.

Cualquiera puede alojar sus imágenes en Docker Hub, de modo que la mayoría de las aplicaciones y las distribuciones de Linux que necesitará tendrán imágenes alojadas allí.

Para **buscar** la imagen de Ubuntu:

```
docker search ubuntu
```

La secuencia de comandos rastreará Docker Hub y mostrará una lista de todas las imágenes cuyo nombre coincida con la cadena de búsqueda.

Para **descargar** la imagen oficial de **Ubuntu** a su equipo:

```
docker pull ubuntu
```

Para ver las **imágenes descargadas** a su PC:

```
docker images
```

**Ejecutar** un contenedor:

```
docker run -it ubuntu
```

Realmente ejecuta un comando en un nuevo contenedor. Este comando **run** primero crea una capa de contenedor en la que se puede escribir sobre la imagen especificada y luego la inicia con el comando especificado.

Este ejemplo, ejecuta un contenedor usando la imagen más reciente de Ubuntu. La combinación de las opciones **-i** y **-t** brindan acceso interactivo del Shell al contenedor.

Ten en cuenta el <mark style="color:blue;">`ID del contenedor`</mark> en el símbolo del sistema. En mi caso es **add8ed81f423.**

Dentro del contenedor puedes ejecutar cualquier comando y no es necesario utilizar el comando **sudo**, ya que realizas las operaciones como el usuario **root**.

Para **ejecutar** un comando en un **contenedor en ejecución**.

```
docker exec
```

Este comando solo se puede utilizar mientras se ejecuta el proceso principal del contenedor y no se reinicia si se reinicia el contenedor.

```
docker exec 270dea23c68b /etc/init.d/nginx start
```

Si probamos a **instalar** una aplicación como Node.js:

```
docker install nodejs
```

**Verificar** que está instalado el Node.js

```
docker -v
```

**Cerrar** el contenedor

```
exit
```

**Ver** solo contenedores **activos**

```
docker ps
```

**Ver** todos los contenedores **activos e inactivos**

```
docker ps -a
```

Ver **último** contenedor creado

```
docker ps -l
```

Para **iniciar** un contenedor detenido, necesitas el ID del contenedor

```
docker start add8ed81f423
```

El contenedor se **iniciará** y podrá ver su estado:

```
docker ps
```

Para **detener** un contenedor en funcionamiento, seguido del ID o nombre del contenedor

```
docker stop add8ed81f423
```

Si ya no necesitas un contenedor, lo puedes **eliminar** y usar nuevamente el ID o el nombre del contenedor.

```
docker rm
```

Puedes utilizar el comando **docker ps -a** para encontrar el ID o nombre del contenedor asociado con la imagen **hello-world** y elimínelo.

```
docker rm affectionate_swanson
```

Puedes iniciar un nuevo contenedor y darle un nombre usando el conmutador&#x20;

```
--name
```

También podrá usar el conmutador de --rm para crear un contenedor que se elimine de forma automática cuando se detenga.

Consulta el comando:&#x20;

```
docker run help
```

para obtener más información sobre estas y otras opciones.

Los contenedores pueden convertirse en imágenes que podrá usar para crear contenedores nuevos. Veamos cómo funciona esto.

Docker brinda un comando que te permite eliminar cualquier recurso: imágenes, contenedores, volúmenes y redes, que no estén asociados con un contenedor:

```
docker system prune
```

Para eliminar adicionalmente los contenedores detenidos y todas las imágenes no utilizadas (no solo aquellas pendientes), añada el indicador `-a` al comando:

```
docker system prune -a
```

#### Confirmar cambios que se han aplicado a una imagen de docker <a href="#h.856756hirmmz" id="h.856756hirmmz"></a>

Al iniciar una imagen de Docker, es posible crear, modificar y eliminar archivos del mismo modo que con una **máquina virtual**.

Los cambios que realices solo se aplican al contenedor sobre el que estás trabajando. De modo que podrás iniciarlo y detenerlo, pero si lo destruyes con el comando <mark style="color:blue;">`docker rm`</mark>, los cambios se perderán por completo.

Veamos como guardar el estado de un contenedor como una nueva imagen de Docker (entiendo que al estilo de una instantánea de Virtual Box).

Dado que hemos instalado Node.js dentro del contenedor de Ubuntu, tenemos un contenedor que se ejecuta a partir de una imagen diferente de la utilizada para crearlo. Sin embargo, veremos como reutilizar este contenedor **Node.js** como base de nuevas imágenes posteriores.

Se tienen que confirmar los cambios en una nueva instancia de imagen de Docker utilizando el siguiente comando:

```
docker commit -m "What you did to the image" -a "Author Name" container_id repository/new_image_name
```

donde:

* **-m** para el mensaje de confirmación que te permite a saber qué cambios realizaron,
* **-a** para especificar el autor.
* **container\_id** el id del contenedor.
* **repository** corresponde a su nombre de usuario de Docker Hub.

Por ejemplo, para el usuario **kirby**, con el ID de contenedor **add8ed81f423**, el comando sería el siguiente:

```
docker commit -m "added Node.js" -a "kirby" add8ed81f423 kirby/ubuntu-nodejs
```

Al _confirmar - confirme -_ una imagen, la nueva se guardará a nivel local en su PC.

Si ahora listamos las imágenes de Docker de nuevo mostrará la nueva imagen, así como la anterior de la que derivó:

```
docker images
```

* En este ejemplo **ubuntu-nodejs** es la nueva imagen, derivada de la imagen **ubuntu** existente de Docker Hub.
* La diferencia de tamaño refleja los cambios realizados. En este ejemplo, el cambio fue la instalación de **NodeJS**.
* Por ello, la próxima vez que deba ejecutar un contenedor usando Ubuntu con NodeJS preinstalado, podrá usar simplemente la nueva imagen.

Nota: También podrá crear imágenes de un Dockerfile, lo cual le permitirá automatizar la instalación de software en una nueva imagen.

En un contenedor interactivo con

```
docker run -it ubuntu
```

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

```
/var/lib/docker/volumes
```

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

```
docker build -t "test-ub-nginx:dockerfile" .
```

En pantalla podemos verificar la ejecución de las instrucciones y finalmente con el comando docker images comprobar que se ha creado la imagen a partir del dockerfile. Esto es:

```
/var/lib/docker/volumes
```

```
docker images

```

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

```
docker login -u docker-registry-username
```

Te solicitará autenticarte usando la contraseña de Docker Hub.



<mark style="color:red;"></mark>
