# Docker compose

Se trata de una utilidad de Docker que nos ayuda a la hora de crear múltiples contenedores Docker al mismo tiempo. Esto es, una herramienta que facilita la <mark style="color:blue;">`orquestación`</mark> local de contenedores**,** con el objetivo de definir y ejecutar aplicaciones Docker de varios contenedores de forma fácil y rápida.

Esta definición y orquestación se lleva a cabo de forma local en el interior de los contenedores**,** que se encuentran unidos a través de una [`red de Docker`](https://keepcoding.io/blog/que-son-las-redes-en-docker/).

A diferencia de <mark style="color:blue;">`Docker Cli`</mark> éste no hay que crearlo directamente en la terminal, sino que se crea un archivo <mark style="color:blue;">`YAML`</mark> que es más fácil de ser modificado y configurado que en la propia terminal.&#x20;

Este tipo de ficheros <mark style="color:blue;">`.yml`</mark>  nos sirven para definir la configuración de la aplicación en cuestión. De esta manera podemos, con un solo comando, crear e iniciar los servicios configurados en estos archivos.

Uno de los aspectos a tener en cuenta es establecer los servicios que componen las aplicación en un fichero [<mark style="color:blue;">`docker`</mark>](https://colaboratorio.net/glosario/docker/)<mark style="color:blue;">`-compose.yml`</mark>, para que se pueda utilizar en un entorno aislado y así, una vez creado, solo se tiene que ejecutar un comando por terminal que comprobará el archivo e instalará y configurará lo que le hemos descrito en el mismo.



![](<../../../../.gitbook/assets/image (177).png>)



Una característica del Docker Compose estriba en su capacidad para contener diversos entornos dentro de una misma máquina de _host_**,** así como su capacidad de mantener y preservar datos de volumen una vez se crean los diferentes contenedores.&#x20;

Veamos algunas de sus características:

_**Multiplicidad de entornos en un solo host.**_ La posibilidad de utilizar un <mark style="color:blue;">`nombre`</mark> (de proyecto), para aislar los entornos entre sí. Dicho nombre se puede utilizar en un _host_ de desarrollo para crear diversas copias de un mismo entorno.

_**Conservar los datos de volumen.**_ Permite la preservación de la información de volumen en los casos donde los contenedores se crean en el sistema; es decir, puede conservar la totalidad de los volúmenes de sus servicios.

_**Recrear contendores que hayan sido modificados.**_ Almacena en caché toda la información relacionada con los ajustes y configuraciones usados para crear un contenedor. En los casos donde un servicio que no ha cambiado se reinicia, Compose se encarga de reutilizar los _contenedores_ que ya existen**.** Esto posibilita que el sistema pueda llevar a cabo cambios dentro de su entorno de forma muy rápida.

## Comandos básicos de docker-compose

Algunos comandos que nos pueden ser útiles a la hora de manipular los contenedores descritosen un archivo de docker-compose.yml&#x20;

* <mark style="color:blue;">`docker-compose up:`</mark> Crea los contenedores (servicios) que están descritos en el <mark style="color:blue;">`docker-compose.yml`</mark>.
* <mark style="color:blue;">`docker-compose up -d`</mark>: Crea en modo detach los contenedores (servicios) que están descritos en el docker-compose.yml y no muestra mensajes de log en el terminal y se  vuelve a mostrar un prompt.
* <mark style="color:blue;">`docker-compose stop`</mark><mark style="color:blue;">:</mark> Detiene los contenedores que previamente se han lanzado con docker-compose up.
* <mark style="color:blue;">`docker-compose run`</mark>: Inicia los contenedores descritos en el docker-compose.yml que estén detenidos.
* <mark style="color:blue;">`docker-compose rm`</mark><mark style="color:blue;">:</mark> Borra los contenedores detenidos. La opción -f elimina también los contenedores en ejecución.
* <mark style="color:blue;">`docker-compose build:`</mark> buscará el archivo <mark style="color:blue;">`docker-compose.yml`</mark> en la carpeta actual, ejecuta el proceso de construcción de la imagen y desplegará los contenedores que estén indicados.&#x20;
* <mark style="color:blue;">`docker-compose down`</mark><mark style="color:blue;">:</mark> Detiene los contenedores, los borra y también elimina las redes que se han creado con docker-compose up (en caso de haberse creado).
* <mark style="color:blue;">`docker-compose down -v`</mark>: Para los contenedores y borra contenedores, redes y volúmenes.
* <mark style="color:blue;">`docker-compose logs`</mark><mark style="color:blue;">:</mark> Muestra los logs de todos los servicios del escenario. Si utilizamos el parámetro <mark style="color:blue;">`-f`</mark> podemos ver los logs en “vivo”.
* <mark style="color:blue;">`docker-compose logs servicio_1`</mark><mark style="color:blue;">:</mark> Muestra los logs del servicio llamado servicio\_1 que estaba descrito en el docker-compose.yml.
*   <mark style="color:blue;">`docker-compose exec servicio_1 /bin/bash`</mark><mark style="color:blue;">:</mark> Ejecuta una orden, en este caso /bin/bash en un contenedor llamado servicio\_1 que estaba descrito en el docker-compose.yml.

    \


### Links

* [https://keepcoding.io/blog/que-es-docker-compose/](https://keepcoding.io/blog/que-es-docker-compose/)
